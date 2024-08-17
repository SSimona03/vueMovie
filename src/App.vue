

<template>


  <main class="p-10 ">
    <div class="wrapper p-10">
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          {{ movie.title }}
        </li>
      </ul>
    </div>
  </main>
</template>

<style scoped>

</style>


<script setup>
import { ref, onMounted } from 'vue';

// Define your API key and URL
const apiKey = import.meta.env.VITE_APP_API_KEY;
const moviesURL = 'https://api.themoviedb.org/3/movie/popular';

// Create a reactive variable to hold the movies data
const movies = ref([]);
console.log(apiKey,"here")
// Fetch movies data when the component is mounted
onMounted(async () => {
  try {
    const response = await fetch(moviesURL, {
      method: 'GET',
      headers: {
        'Authorization': `${apiKey}`,
        'Accept': 'application/json',
      }
    });

    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    const data = await response.json();
    movies.value = data.results; // Set the movies data
  } catch (error) {
    console.error('Error fetching movies:', error);
  }
});
</script>