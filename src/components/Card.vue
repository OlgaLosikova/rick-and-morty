<script setup>
defineProps({
  name: String,
  status: String,
  species: String,
  location: String,
  origin: String,
  imgUrl: String,
});
</script>

<template>
  <article class="card">
    <img class="card-image" alt="character" :src="imgUrl" />
    <div class="img-text">
      <h2>{{ name }}</h2>
      <div class="status-wrapper">
        <span
          :class="
            status === 'Dead' ? 'card-point card-point-danger' : 'card-point'
          "
        ></span
        ><span>{{ status }} - {{ species }}</span>
      </div>
      <span class="card-subtitle">Location:</span>
      <span>{{ location }}</span>
      <span class="card-subtitle">Origin:</span>
      <span>{{ origin }}</span>
    </div>
  </article>
</template>

<style scoped>
.card {
  background-color: #666;
  border-radius: 10px;
  display: flex;
  height: 220px;
  overflow: hidden;
  padding-right: 1rem;
}
.card-image {
  border-radius: 10px 0 0 10px;
  padding-right: 1rem;
  object-fit: contain;
}
.img-text {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-evenly;
}
.card-point {
  background-color: rgb(50, 182, 50);
  width: 8px;
  height: 8px;
  margin-right: 5px;
  border-radius: 3px;
}
.card-point-danger {
  background-color: rgb(255, 60, 0);
}
.status-wrapper {
  display: flex;
  align-items: center;
}
.card-subtitle {
  color: #ccc;
  font-size: 0.9rem;
  margin-top: 0.5rem;
}
</style>
