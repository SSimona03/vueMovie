<template>
  <main class="p-10 ">
    <div class="wrapper p-10">
      <ul class="flex flex-wrap justify-around">
        <li v-for="movie in movies" :key="movie.id" class="w-64 p-3">
          <img 
            :src="imgBase + movie.poster_path" 
            :alt="movie.title + ' poster'" 
            class="cursor-pointer"
            @click="showModal(movie)"
          />
          <p class="font-sans text-lg font-medium mt-3">{{ movie.title }}</p> 
          <p class="font-sans text-sm">Vote: {{ parseFloat(movie.vote_average.toFixed(1)) }}</p>
        </li>
      </ul>
    </div>

    <!-- Modal -->
    <div v-if="isModalVisible" class=" mx-6 fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
      <div class="bg-cyan-950 p-8 rounded-lg shadow-lg text-center">
        <h2 class="text-2xl font-bold mb-4">{{ selectedMovie.title }}</h2>
        <div class="flex-col flex justify-center md:flex-row mx-auto	 ">
        <img 

            :src="imgBase + selectedMovie.poster_path" 
            :alt="selectedMovie.title + ' poster'" 
            class="w-32 md:w-44 mx-auto pb-3"
           
          />
          <div>
            <p class="mx-12  pb-3 md:text-left">Release Date: <span class="font-semibold">{{ selectedMovie.release_date }}</span></p>
            <p class=" md:mx-12 text-left">{{ selectedMovie.overview }}</p>
          </div>
       
      </div>
        <button @click="closeModal" class="w-full mt-4 px-4 py-2 bg-blue-500 text-white rounded md:w-44 ">Close</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}
</style>

<script setup>
import { ref, onMounted } from 'vue';

const apiKey = import.meta.env.VITE_APP_API_KEY;
const moviesURL = 'https://api.themoviedb.org/3/movie/popular';
const imgBase = "https://media.themoviedb.org//t/p/w440_and_h660_face/";

// Create reactive variables to hold the movies data and modal state
const movies = ref([]);
const isModalVisible = ref(false);
const selectedMovie = ref('');

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

// Function to show modal and set the selected movie
const showModal = (movie) => {
  selectedMovie.value = movie; // Store the entire movie object
  isModalVisible.value = true;
};

// Function to close the modal
const closeModal = () => {
  isModalVisible.value = false;
};
</script>
