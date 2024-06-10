<script setup>
defineProps({
  pageNumber: Number,
  setNextPage: Function,
  setPrevPage: Function,
  disabledNext: Boolean,
  disabledPrev: Boolean,
  empty:Boolean,
});
</script>

<template>
  <footer v-show="!empty" class="footer">
    <button @click="setPrevPage" :disabled="disabledPrev">Prev</button
    ><span class="current-page">{{ pageNumber }}</span>
    <button @click="setNextPage" :disabled="disabledNext">Next</button>
  </footer>
</template>

<style scoped>
.footer {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 2.5rem 0 2.5rem;
}
.current-page {
  margin-left: 1rem;
}
</style>
