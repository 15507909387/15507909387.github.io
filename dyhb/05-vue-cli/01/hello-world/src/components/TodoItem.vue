<template>
  <li>
    <input type="checkbox">
    <p class="">{{todo.text}}</p>
    <span>X</span>
  </li>
</template>

<script>
export default {
  props:["todo"]
}
</script>

<style scoped>

 ul li p {
   margin: 0 10px;
 }

 ul li span {
  color:rgb(255, 94, 0);
  
}
</style>
