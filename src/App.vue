<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'
export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
  @import url('./style/reset.css');
  #app{
    display: flex;
    height: 100vh;
    width: 100vw;
    max-width: 530px;
  }
</style>
