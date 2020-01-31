<template>
  <div id="app">
    <p>{{msg}}</p>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {

  },
  data() {
    return {
      msg: 'Hello',
      todos: [
        {
          id: 0,
          title: 'todo 1',
          completed: false,
        },
        {
          id: 1,
          title: 'todo 2',
          completed: false,
        },
        {
          id: 2,
          title: 'todo 3',
          completed: false,
        },
      ]
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
</style>
