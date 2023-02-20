<template>
  <div class="container">
    <h1 class="title">todos</h1>
    <TodoList />
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";
export default {
  name: "App",
  components: {
    TodoList,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}
.title {
  margin-top: 50px;
  display: flex;
  justify-content: center;
}
#app{
  font-family: Arial, Helvetica, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
</style>
