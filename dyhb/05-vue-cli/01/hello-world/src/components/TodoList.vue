<template>
  <ul>
    <TodoItem v-for="(todo, index) in todos" :todo="todo" :key="index" />
  </ul>
</template>

<script>
import TodoItem from "./TodoItem.vue";

export default {
  props: ["todos"],
  components: {
    TodoItem
  }
}
</script>

<style scoped>
ul li {
  width: 240px;
  
  display: flex;
  align-items: center
}


</style>
