# Vue components
## Overview
Vue components are templates rather than full on components. Templates let you get even closer to what might get rendered by the HTML engine while still allowing you to add interactivity via whatever Domain Specific Language Vue uses. This is in contrast to React components, where they're written as either JavaScript functions, or ES6 classes.

See the [Vue docs](https://vuejs.org/v2/guide/comparison.html#HTML-amp-CSS) for a comparison.

## Vue Template Syntax
So if we check out the `App.vue` component, we see that it has three sections like so:
```html
<template>
  <template-here/>
</template>

<script>
  doJavaScriptHere()
</script>

<style>
body {
  color: pink;
}
</style>
```

The `<template>` section is the HTML markup that would be analogous to JSX in React.

The `<script>` tag is where you start adding interactivity, imports and what the template exports.

The `<style>` section is where you add styling/ CSS and you can either scope that styling to the component, or have the styling be global.

So we'll note that in the pre-generated `App.vue` we use a `HelloWorld` component in the template like so:
```xml
<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>
```

This HelloWorld component needs to be imported in the `scripts` section and exported as a named dependency like so:
```html
<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>
```

And if we look back at our template, we'll see that the `HelloWorld` component gets a "msg" prop passed in like we would in React.

## Vue Component Props
We're going to look more into the `HelloWorld` component now.

Previously we talked about component props. Vue components can get props just like React components, but they operate a little differently.

In Vue, we consume input props in the template by using double curly brackets like so: `{{ my-prop-name }}`

Then we need to declare that prop inside the script like so:
```html
<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>
```

Just like in `App` we can add a props object into the default export.

## Shorthand syntax
Vue actually has shorthand syntax for a whole bunch of stuff, one for `v-bind` and one for `v-on`.

Whereas before for binding a key value, we'd need:
```xml
  <div v-bind:key="3">
  </div>
```

We can just remove the `v-bind` portion and just identify the attribute we want to bind a value to like so:
```xml
  <div :key="3">
  </div>
```

Same with `v-on`:
```xml
  <!-- Longhand -->
  <form v-on:submit="submitForm">
  </form>

  <!-- Shorthand -->
  <form @submit="submitForm">
  </form>
```

See the docs on [Syntax Shorthand](https://vuejs.org/v2/guide/syntax.html#Shorthands) for more.

## Vue Slots
### Syntax Overview
Vue slots function in a very similar way as the [Web Components spec](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) for them. It also sort of works the same way as `props.children` in React only in Vue, it's closer to how you'd expect HTML to work as you can stick arrays and objects into a ReactNode.

Slots in Vue let us do some wacky composition stuff, and it gets turned up to eleven in Gridsome.

For example, given the below navigation link component:
```xml
  <navigation-link url="/profile">
    Your Profile
  </navigation-link>
```

Within the actual `<navigation-link>` component, we have:
```xml
  <a
    v-bind:href="url"
    class="nav-link"
  >
    <slot></slot>
  </a>
```

So the `<slot>` gets replaced with the "Your Profile" text.

### Compilation Scope
The slot has access to any data declared inside the `<navigation-link>` component, BUT will not have access to any data that is **passed into** to the `<navigation-link>`.

So in the above example, we **do not** have access to the "/profile" value passed into our navigation link.

See the [Vue slots docs](https://vuejs.org/v2/guide/components-slots.html) for more.