<script setup>
import { onMounted, reactive, ref } from "vue";
import axios from "axios";
import Card from "./components/Card.vue";
import Header from "./components/Header.vue";

const baseUrl = "https://rickandmortyapi.com/api";
const characters = ref([]);
const error = ref(null);
const filters = reactive({
  selectStatus: "all",
  search: "",
});
const changeSelect = (e) => {
  filters.selectStatus = e.target.value;
};
const changeInput = (e) => {
  filters.search = e.target.value;
};
const fetchItems = async () => {
  try {
    const params = {};
    if (filters.selectStatus !== "all") {
      params.status = `${filters.selectStatus}`;
    }
    if (filters.search) {
      params.name = `${filters.search}`;
    }
    const { data } = await axios.get(`${baseUrl}/character`, { params });
    characters.value = data.results;
    error.value = null
  } catch (err) {
    error.value = "There is nothing here";
  }
};
onMounted(fetchItems);
</script>

<template>
  <Header
    :changeSelect="changeSelect"
    :changeInput="changeInput"
    :fetchItems="fetchItems"
  />
  <main>
    <h1>The Rick and Morty</h1>
    <p class="error" v-if="error">{{ error }}</p>
    <section class="card-wrapper">
      <Card
        v-if="!error"
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
.error {
  color: red;
  font-size: 2rem;
  text-align: center;
}
</style>
