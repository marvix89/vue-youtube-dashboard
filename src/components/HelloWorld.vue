<script setup>
import { ref,watchEffect } from 'vue'


defineProps({
  msg: {
    type: String,
    required: true
  }
})

const data = ref(null)
const loading = ref(false)
const query = ref('')

const fetchData = async () => {
  loading.value = true
  try {
    const response = await fetch(`https://www.googleapis.com/youtube/v3/search?part=snippet&q=${query.value}&key=AIzaSyB4sxeGx0GWQmacHoca67Raiij8J7LxdvA`)
    data.value = await response.json()
  } catch (error) {
    console.error(error)
  } finally {
    loading.value = false
  }
}

watchEffect(() => {
  if (query.value) {
    fetchData()
  }
})
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <input v-model="query" type="text" placeholder="Search YouTube">
    <button @click="fetchData">Search</button>
  <div v-if="loading">
      Loading...
    </div>
    <div v-else-if="data && data.items">
      <div v-for="item in data.items" :key="item.id.videoId" class="card">
        <img :src="item.snippet.thumbnails.default.url" alt="Thumbnail">
        <h2>{{ item.snippet.title }}</h2>
        <p>{{ item.snippet.description }}</p>
        <a :href="'https://www.youtube.com/watch?v=' + item.id.videoId" target="_blank">Watch on YouTube</a>
      </div>
    </div>
    <div v-else>
      No results found.
    </div>
  </div>
</template>

<style scoped>


</style>
