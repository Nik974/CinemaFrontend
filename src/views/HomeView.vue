<template>
  <div class="home-container">
    <NavBar />

    <div class="content">
      <h1>Repertuar</h1>

      <div v-if="loading" class="loading">Ładowanie filmów...</div>

      <div v-else class="movies-grid">
        <MovieCard
          v-for="film in films"
          :key="film.id"
          :movie="film"
          @buy="goToDetails"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import NavBar from '../components/NavBar.vue';
import MovieCard from '../components/MovieCard.vue';

const router = useRouter();
const films = ref([]);
const loading = ref(true);

const fetchFilms = async () => {
  try {
    const response = await axios.get('http://localhost:8080/api/films');
    films.value = response.data;
  } catch (error) {
    console.error('Błąd pobierania filmów:', error);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchFilms();
});


const goToDetails = (filmId) => {

  console.log('Wybrano film:', filmId);

  alert(`Tu będą seanse dla filmu ID: ${filmId}`);
};
</script>

<style scoped>

.home-container { min-height: 100vh; background-color: #f4f4f4; }
.content { max-width: 1200px; margin: 0 auto; padding: 20px; }
h1 { text-align: center; color: #2c3e50; }
.movies-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 30px; }
.loading { text-align: center; font-size: 20px; margin-top: 50px; }
</style>
