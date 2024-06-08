<script setup>
defineProps({
  changeSelect: Function,
  changeInput: Function,
  fetchItems:Function
});
</script>

<template>
  <header class="header">
    <input
      class="search"
      type="search"
      placeholder="Найти..."
      @input="changeInput"
    /><select class="select" @change="changeSelect">
      <option value="all">All</option>
      <option value="alive">Alive</option>
      <option value="dead">Dead</option>
      <option value="unknown">Unknown</option>
    </select>
    <button @click="fetchItems">Применить</button>
  </header>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: center;
  padding: 0 2.5rem 0 2.5rem;
}
.search {
  width: 400px;
  margin-right: 1.5rem;
  height: 36px;
  font-size: 1rem;
}
.select {
  font-size: 1rem;
}
</style>
