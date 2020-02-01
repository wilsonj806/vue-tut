# Dynamic Vue Components
## Overview
Vue handles dynamic components differently since it runs via templating.

## Basics
First, as mentioned before, every vue component has a default export in the scripts section like so:

```html
<script>
import Todos from './components/Todos'

export default {
  name: 'app',
  components: {
    Todos
  },
  props: {

  },
  data() { return { }}
}
</script>
```

The default export is an object that can contain several things that relate to how the Vue template is rendered. Below is a list/ brief description of them:

1) "name" is the name of the component, pretty straight-forwards
2) "components" is a list of any other component that the template imports/ depends on in order to render
3) "props" is an object that declares the various input props that this template can expect. It functions similarly to Prop-Types if you don't know what the prop value is ahead of time
4) "data" is a function that returns an object with the data that your component needs in order to run. This is where you'd do data fetching and stuff.


## Styling
As mentioned by the [Basic Vue Component Syntax doc](./vue-component-syntax.md), we can also have styling, and said styling can be scoped to the component via the `scoped` flag like so:

```html
<style scoped>
div {
  color: pink;
}
</style>
```

## Vue Directives
Vue directives are the Domain Specific Language for handling dynamic data/ data passing

### Data binding
If we want to pass data along to another component, then we'd need to bind that data into that component with the `v-bind` like so:
```xml
<template>
  <div>
    <Todos v-bind:todos="todos"/>
  </div>
</template>
```

And the syntax for it is: `v-bind:<prop-name-in-child> ="<prop-value-from-parent>"`

`v-bind` functionally just binds a new attribute with the desired name and value, to the desired function. It also works on existing attributes like `class` or `href`

We can then use that value in the `Todos` component as a prop like so:

```html
<script>
export default {
  name: 'Todos',
  props: [
    "todos"
  ]
}
</script>
```

### Consuming Passed Data And Looping
Our data is available in the `Todos` component, and we can declare the prop as a dependency in the default export, but it's an array of data, not just like one singular string.

To consume the **array of data**, we need to use the `v-for` directive. Looks like the below:
```html
  <div v-for="todo in todos" v-bind:key="todo.id">
    <h3>{{ todo.title }}</h3>
  </div>
```

The syntax is pretty funky because of the `vfor="todo in todos"` but it semantically works out if you interpret it like a forEach sort of thing like:
- vue directive: for each todo in the todos array => render stuff in the children markup

In addition to `v-for`, we also need to bind a key to each child element just like React requires it. We thus need to add `v-bind:key` in.

## Vue Events
Event handling works a bit differently in Vue. Lookss like the below for `TodoItem` in the template:
```xml
<template>
  <div class="todo-item" v-bind:class="{'is-complete':todo.completed}">
    <input type="checkbox" v-on:change="markComplete"/>
    <p>{{ todo.title }}</p>
  </div>
</template>
```

Where we use the `v-on:` directive and identify the event as `change`, and we also add the event handler name which is "markComplete".

Of course this event handler doesn't exist, so we add it into our scripts section like so:

```html
<script>
export default {
  name: 'TodoItem',
  props:[
    'todo'
  ],
  methods: {
    markComplete() {
      this.todo.completed = !this.todo.completed
    }
  },
}
</script>
```

That's for one off events that are local to that particular compmonent, but what if we want to do stuff with that event from higher up the component tree? In this situation, if we want to delete a todo, we'd need to delete it from the App as well. To do this, we use event emitters and it looks like the below:

```xml
<template>
  <div class="todo-item" v-bind:class="{'is-complete':todo.completed}">
      <!-- additional markup not included -->
      <!-- registering a click event and emitting the event-->
      <button class="del" type="button" @click="$emit('del-todo', todo.id)">x</button>
  </div>
</template>
```

Where we have a button element with an `@click` attribute with a value of `$emit()`. And what we're emitting is a Vue event called "del-todo" with the todo.id as the value.

After we do that, we need to catch the event higher up in the tree and it looks like so in Todos and eventually App:

```xml
<!-- Todo -->
<template>
  <div>
    <!-- Renders todos.length number of divs with a unique key -->
    <div v-for="todo in todos" v-bind:key="todo.id">
      <TodoItem v-bind:todo="todo" v-on:del-todo="$emit('del-todo', todo.id)"/>
    </div>
  </div>
</template>
```

In the Todo component we add `v-on:`, then the event name, and then the name of the callback we want to call. The Todo component needs to then emit it's own event so that App can modify state.

```xml
<!-- App -->
<template>
  <div id="app">
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>
```

And we add a deleteTodo method to the default export like so:

```html
<script>
import Todos from './components/Todos'

export default {
  name: 'app',
  components: {
    Todos
  },
  data() {
    return {...stuff}
  },
  methods: {
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    }
  }
}
</script>
```

So when we click on the delete button on the todo, it deletes it from the app data.

## Interactivity
We've got events, which is great but it's not super interactive just yet. To demo this, we're going to make our `AddTodo` component which does exactly that. A basic AddTodo component has an input and a submit button, so we'll need some way to update the value of the input as we go, and we need to have a submit handler.

For the input, we first need to define it's value as part of the component's data like so:

```html
<script>
export default {
  name: "AddTodo",
  data() {
    return {
      title: ''
    }
  }
}
</script>
```

And then we connect our data to our template using `v-model` like so:
```xml
<template>
  <div>
    <form>
      <input v-model="title" type="text" name="title" placeholder="Add Todo"/>
      <input class="btn" type="submit" value="Submit"/>
    </form>
  </div>
</template>
```

Where the value for `v-model` corresponds to the title property returned by `data()`.

So that handles input changes, now for form submission. It functions the same way as a click event:

```xml
<template>
  <div>
    <form @submit="addTodo">
      <input v-model="title" type="text" name="title" placeholder="Add Todo"/>
      <input class="btn" type="submit" value="Submit"/>
    </form>
  </div>
</template>
```

We aren't pushing this up to the parent container just yet as we want to do some data processing like so:

```html
<script>
import uuid from 'uuid'

export default {
  name: "AddTodo",
  data() {
    return {
      title: ''
    }
  },
  methods: {
    addTodo() {
      const newTodo = {
        id: uuid.v4(),
        title: this.title,
        completed: false
      };

      // Send new todo up to the parent
      this.$emit('add-todo', newTodo);
    }
  }
}
</script>
```