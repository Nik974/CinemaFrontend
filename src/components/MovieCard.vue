<template>
  <div class="movie-card">
    <div class="poster" :style="{ backgroundImage: `url(${getImageUrl(movie)})` }"></div>

    <div class="info">
      <h3>{{ movie.title }}</h3>
      <p class="meta">{{ movie.year }} | {{ movie.duration }} min</p>

      <p class="description">{{ truncate(movie.description, 60) }}</p>

      <button @click="$emit('buy', movie.id)">Zobacz seanse</button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  movie: {
    type: Object,
    required: true
  }
});


const getImageUrl = (movie) => {

  return `https://picsum.photos/seed/${movie.id}/300/450`;
};

const truncate = (text, length) => {
  if (!text) return '';
  return text.length > length ? text.substring(0, length) + '...' : text;
};
</script>

<style scoped>
.movie-card {
  background: white; border-radius: 10px; overflow: hidden;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1); transition: transform 0.2s;
  display: flex; flex-direction: column; height: 100%;
}
.movie-card:hover { transform: translateY(-5px); }
.poster { height: 350px; background-size: cover; background-position: center; }
.info { padding: 15px; flex-grow: 1; display: flex; flex-direction: column; }
h3 { margin: 0 0 5px 0; font-size: 18px; color: #333; }
.meta { font-weight: bold; color: #666; font-size: 14px; margin-bottom: 10px; }
.description { font-size: 13px; color: #777; margin-bottom: 15px; flex-grow: 1; }
button {
  width: 100%; background-color: #2c3e50; color: white; border: none;
  padding: 10px; border-radius: 5px; cursor: pointer; font-weight: bold; margin-top: auto;
}
button:hover { background-color: #42b983; }
</style>
