<script setup>
import { onMounted, reactive, ref } from "vue";
import axios from "axios";
import Card from "./components/Card.vue";
import Header from "./components/Header.vue";
import PaginateCharacters from "./components/PaginateCharacters.vue";

const baseUrl = "https://rickandmortyapi.com/api";
const characters = ref([]);
const error = ref(null);
const pageInfo = reactive({});
const page = reactive({
  pageNumber: 1,
  disabledNext: false,
  disabledPrev: true,
  empty: false,
});
const filters = reactive({
  selectStatus: "all",
  search: "",
  disabled: true,
});
const changeSelect = (e) => {
  filters.selectStatus = e.target.value;
  filters.disabled = false;
};
const changeInput = (e) => {
  filters.search = e.target.value;
  filters.disabled = false;
};
const setNextPage = () => {
  if (page.pageNumber < pageInfo.value.pages) {
    page.pageNumber++;
    fetchItems();
  }
};
const setPrevPage = () => {
  if (page.pageNumber > 1) {
    page.pageNumber--;
    fetchItems();
  }
};
const fetchItems = async () => {
  try {
    const params = { page: `${page.pageNumber}` };

    filters.selectStatus !== "all" &&
      (params.status = `${filters.selectStatus}`);
    filters.search && (params.name = `${filters.search}`);

    (params.status || params.name) &&
      !filters.disabled &&
      (params.page = 1) &&
      (page.pageNumber = 1);

    const { data } = await axios.get(`${baseUrl}/character`, { params });
    characters.value = data.results;
    error.value = null;
    pageInfo.value = data.info;

    pageInfo.value.prev
      ? (page.disabledPrev = false)
      : (page.disabledPrev = true);
    pageInfo.value.next
      ? (page.disabledNext = false)
      : (page.disabledNext = true);
      page.empty=false;
  } catch (err) {
    error.value = "There is nothing here";
    page.empty=true;
  } finally {
    filters.disabled = true;
  }
};
onMounted(fetchItems);
</script>

<template>
  <Header
    :changeSelect="changeSelect"
    :changeInput="changeInput"
    :fetchItems="fetchItems"
    :disabled="filters.disabled"
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
    <PaginateCharacters
      :setPrevPage="setPrevPage"
      :setNextPage="setNextPage"
      :disabledNext="page.disabledNext"
      :disabledPrev="page.disabledPrev"
      :pageNumber="page.pageNumber"
       :empty="page.empty"
    />
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
