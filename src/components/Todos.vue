<template>
  <div>
    <ul class="todos">
        <li v-bind:key="todo.uuid" :data-id="todo.uuid" v-for="todo in todos">
        <Todo v-bind:todo="todo" v-on:delete-todo="$emit('delete-todo', todo.uuid)" v-on:complete-todo="$emit('complete-todo', todo.uuid)"/>
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