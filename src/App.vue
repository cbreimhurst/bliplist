<template>
  <div id="app" v-on:mode="toggle" :class="mode">
    <Header :mode="mode" @toggle="toggle" />
    <Todos v-bind:todos="todos" v-on:delete-todo="deleteTodo"/>
    <AddTodo v-on:add-todo="addTodo"/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Todos from './components/Todos.vue';
import AddTodo from './components/AddTodo.vue';


export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      mode: 'light',
      todos: [
        {
          id: 1,
          list_id: 1,
          title: 'Go workout',
          task_text: null,
          completed: false,
          insert_time: null,
          updated_time: null
        },
        {
          id: 2,
          title: 'Do laundry',
          completed: true
        },
        {
          id: 3,
          title: 'Cook food',
          completed: false
        },
        {
          id: 4,
          title: 'Clean up room',
          completed: false
        },
        {
          i: 5,
          title: 'Finish work',
          completed: false
        }
      ],
    }
  },
  methods: {
    addTodo(newTodoObj) {
      this.todos = [...this.todos, newTodoObj];
    },
    deleteTodo(todoId) {
      this.todos = this.todos.filter(todo => todo.id !== todoId);
    },
    toggle() {
      if (this.mode === "dark") {
        this.mode = "light"
      } else {
        this.mode = "dark"
      }
    }
  },
  mounted() {
    if (localStorage.mode) {
      this.mode = localStorage.mode;
    }
  },
  watch: {
    mode(newMode) {
      localStorage.mode = newMode;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: #f4f3f0;
  color: #2c3e50;
  transition: background 0.3s ease-in-out, color 0.3s ease-in-out;
  padding: 15px;
  min-height: 100vh;
}

#app.dark {
  background: #192734;
  color: #f4f3f0;
}

body{
  margin: 0 ;
}

  body,
      input[type="text"],
      textarea {
        font-size: 16px;
      }

      button {
        background-color: #00d1b2;
        border-color: transparent;
        color: #fff;
        cursor: pointer;
        justify-content: center;
        padding-bottom: calc(0.5em - 1px);
        padding-left: 0em;
        padding-right: 0em;
        padding-top: calc(0.5em - 1px);
        text-align: center;
        white-space: nowrap;
        border-width: 1px;
        border-radius: 4px;
        box-shadow: none;
        display: inline-flex;
        font-size: 1rem;
        height: 2.5em;
        line-height: 1.5;
        position: relative;
      }

      .todos {
        margin: 0 auto;
        padding: 0;
        list-style: none;
        text-align: left;
        max-width: 400px;
      }

      form {
        margin: 0 auto;
        padding: 0;
        text-align: left;
        max-width: 400px;
        display: flex;
      }

      form button {
        margin-left: auto;
        border-top-left-radius: 0px;
        border-bottom-left-radius: 0px;
      }

      .todos li {
        margin-bottom: 1em;
      }

      .todos li div {
        display: flex;
      }

      .todos li div button {
        margin-left: auto;
        background-color: #f14668;
      }

      input {
        width: 100%;
        padding: 0.4em;
        border-top-left-radius: 4px;
        border-bottom-left-radius: 4px;
        border: 1px solid #00d1b2;
      }

      button svg {
        height: 25px;
      }
</style>
