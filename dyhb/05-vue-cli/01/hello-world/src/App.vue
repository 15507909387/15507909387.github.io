<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld msg="一切发动" />
    <MyTest></MyTest> -->
    <TodoAdd @add-todo="addTodo" />
    <TodoList :todos="todos" />
    <TodoFilter />
  </div>
</template>

<script>
// 注册组件
import TodoAdd from "./components/TodoAdd.vue";
import TodoList from "./components/TodoList.vue";
import TodoFilter from "./components/TodoFilter.vue";

export default {
  name: "App",
  // 组件
  components: {
   
    TodoAdd,
    TodoList,
    TodoFilter,
  },
  data: function() {
    return {
      todos: [
        { text: "学习 JavaScript", completed: true },
        { text: "学习 Vue", completed: false },
        { text: "整个牛项目", completed: true },
      ],
    };
  },
  methods: {
    addTodo: function(text) {
      this.todos.push({
        text,completed:false
      })
    }
  }
  
};


</script>

<style scoped>
 ul {
   list-style: none;
   padding: 0;
   margin: 0;
 }
</style>

