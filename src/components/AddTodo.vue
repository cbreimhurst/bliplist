<template>
  <div>
    <form @submit="addTodo">
      <input 
      type="text" 
      v-model="title" 
      name="title" 
      spellcheck="false" 
      autocomplete="off"
      autocorrect="off"
      autocapitalize="off"
      >
      <button type="submit"><svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-plus" width="44" height="44" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
  <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
  <line x1="12" y1="5" x2="12" y2="19" />
  <line x1="5" y1="12" x2="19" y2="12" />
</svg></button>
    </form>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';



export default {
  name: 'AddTodo',
  data() {
    return {
      title: ''
    }
  },
  methods: {
    addTodo(e) {
      e.preventDefault();      
      const newTodoObj = {
        uuid: uuidv4(),
        title: this.title,
        completed: 0
      }      
      this.$emit('add-todo', newTodoObj);
      this.title = '';
    },
  }
}
</script><style scoped>
</style>