<template>
  <div id="app" v-on:mode="toggle" :class="mode">
    <Header :mode="mode" @toggle="toggle" />
    <Todos v-bind:todos="todos" v-on:delete-todo="deleteTodo" v-on:complete-todo="markComplete"/>
    <AddTodo v-on:add-todo="addTodo"/>


  </div>
</template>

<script>
import Header from './components/Header.vue'
import Todos from './components/Todos.vue';
import AddTodo from './components/AddTodo.vue';


import { createClient } from '@supabase/supabase-js'
const supabaseUrl = 'https://ezobnhwtsnemtgajfsce.supabase.co'
const supabaseKey = process.env.VUE_APP_SUPABASE_KEY;
const supabase = createClient(supabaseUrl, supabaseKey)

export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      title: '',
      mode: 'light',
      todos: [],
    }
  },
  async created() {
    let { body } = await supabase.from("tasks").select("*");
    this.todos = body;
  },
  methods: {

    async addTodo(newTodoObj) {
      //console.log(newTodoObj);
      await supabase.from("tasks").insert([newTodoObj]);
    },
    async deleteTodo(id) {
      console.log(id)
      // this.todos = this.todos.filter(todo => todo.uuid !== todoId);
       await supabase
      .from('tasks')
      .delete()
      .eq('uuid', id)
    },
    async markComplete(todoId) {
      // this function needs work - can be reduced


      let todosObj = this.todos;
      var todoItem = todosObj.find(function(todo) {
        if(todo.uuid == todoId)
          return true;
      });
      todoItem.completed = !todoItem.completed
      let complete = !todoItem.completed

      console.log(complete)
      await supabase
        .from('tasks')
        .update({ completed: complete })
        .eq('uuid', todoId)

      // localStorage.todos = JSON.stringify(this.todos);
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
    // if (localStorage.todos) {
    //   this.todos = JSON.parse(localStorage.todos);
    // }

    supabase
    .from('tasks')
    .on('INSERT', payload => {
      console.log('Change received!', payload)
      this.todos.push(payload.new);
    })
    .on('UPDATE', payload => {
      console.log('Change received!', payload)
      
    })
    .on("DELETE", payload => {
        const id = payload.old.id;
        const index = this.todos.map(x => x.id).indexOf(id);
        this.todos.splice(index, 1)
      })
    .subscribe();

  },
  watch: {
    mode(newMode) {
      localStorage.mode = newMode;
    },
    // todos(newTodos) {
    //   localStorage.todos = JSON.stringify(newTodos);
    // }
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
        max-width: 500px;
      }

      form {
        margin: 0 auto;
        padding: 0;
        text-align: left;
        max-width: 500px;
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

      .todos li div button.edit {
        margin-left: auto;
        background-color: #3273dc;
      }

      .todos li div button.delete {
        margin-left: 5px;
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
