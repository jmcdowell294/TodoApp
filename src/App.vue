<script setup>
import todoList from './components/todoList.vue'
</script>

<template>
  <main class="wrapper">
    <div class="todo">
      <todoList />
    </div>
  </main>
</template>

<style scoped>
.wrapper {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: grid;
  grid-template-columns: 10rem 1fr 10rem;
  grid-template-rows: 8rem 1fr 8rem;
}

.todo {
  grid-column: 2;
  grid-row: 2;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
}
</style>
