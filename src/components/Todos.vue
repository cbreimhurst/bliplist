<template>
  <div>
    <ul class="todos">
        <li v-bind:key="todo.id" v-for="todo in todos">
        <Todo v-bind:todo="todo" v-on:delete-todo="$emit('delete-todo', todo.id)" v-on:complete-todo="$emit('complete-todo', todo.id)"/>
      </li>
    </ul>
  </div>
</template>
<script>
import Todo from './Todo.vue';
export default {
  name: 'Todos',
  components: {
    Todo
  },
  props: [
    "todos"
  ]
}
</script><style scoped>
</style>