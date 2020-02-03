<template>
  <div class="home">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos'
import AddTodo from '../components/AddTodo'

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo,
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      const fetchOpt = {
        method: 'DELETE',
      }
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`, fetchOpt)
        .then(() => this.todos = this.todos.filter(todo => todo.id !== id))

    },
    addTodo(newTodo) {
      // old non HTTP version
      // this.todos = [...this.todos, newTodo]
      const { title, completed } = newTodo
      const fetchOpt = {
        method: 'POST',
        headers: {
          "Content-type": "application/json; charset=UTF-8"
        },
        body: JSON.stringify({ title, completed })
      }
      fetch('https://jsonplaceholder.typicode.com/todos', fetchOpt)
        .then(res => res.json())
        .then(json => this.todos = [...this.todos, json])

    }
  },
  // Basically componentDidMount but for Vue
  created() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then(res => res.json())
      .then(json => this.todos = json)
      // eslint-disable-next-line
      .catch(err => console.error(err));
  }
}
</script>
