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