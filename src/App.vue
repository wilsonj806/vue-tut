<template>
  <div id="app">
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from './components/Todos'

export default {
  name: 'app',
  components: {
    Todos
  },
  data() {
    return {
      todos: [
        {
          id: 0,
          title: 'todo 1',
          completed: false,
        },
        {
          id: 1,
          title: 'todo 2',
          completed: true,
        },
        {
          id: 2,
          title: 'todo 3',
          completed: false,
        },
      ]
    }
  },
  methods: {
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
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
