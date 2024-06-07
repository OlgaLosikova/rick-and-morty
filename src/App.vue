<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import axios from "axios";
import Card from "./components/Card.vue";
import Header from "./components/Header.vue";

const baseUrl = "https://rickandmortyapi.com/api";
const characters = ref({});
const filters = reactive({
  selectStatus: "",
  search: "",
});
const changeSelect = (e) => {
  e.target.value === "all"
    ? (filters.selectStatus = "")
    : (filters.selectStatus = e.target.value);
};
onMounted(async () => {
  try {
    const { data } = await axios.get(`${baseUrl}/character`);
    characters.value = data.results;
  } catch (err) {
    console.log(err);
  }
});
watch(filters, async () => {
  try {
    const { data } = await axios.get(
      `${baseUrl}/character/?status=${filters.selectStatus}`
    );
    characters.value = data.results;
  } catch (err) {
    console.log(err);
  }
});
</script>

<template>
  <Header :changeSelect="changeSelect" />
  <main>
    <h1>The Rick and Morty</h1>
    <section class="card-wrapper">
      <Card
        v-for="character in characters"
        :key="character.id"
        :name="character.name"
        :status="character.status"
        :species="character.species"
        :location="character.location.name"
        :origin="character.origin.name"
        :imgUrl="character.image"
      />
    </section>
  </main>
</template>

<style scoped>
.card-wrapper {
  display: grid;
  gap: 2rem;
  grid-template-columns: repeat(auto-fill, minmax(480px, 1fr));
  justify-content: stretch;
  margin: 2.5rem;
}
</style>
