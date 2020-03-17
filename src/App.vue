<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/todoList/todoList'
export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
#app{
  margin-top: 125px;
}
</style>
