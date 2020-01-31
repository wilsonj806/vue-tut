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