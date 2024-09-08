<template>
    <div v-if="anime && anime.title"> <!-- Ensure anime object exists before rendering -->
      <div class="anime-card">
        <!-- Display the anime image -->
        <img :src="anime.imageUrl" alt="Anime Image" class="anime-image" v-if="anime.imageUrl" />
        
        <!-- Display the anime title and other details -->
        <div class="anime-info">
          <p><img :src="anime.images.webp.image_url" /></p>
          <h2>{{ anime.title }}</h2>
          <p>Episodes: {{ anime.episodes }}</p>
          <p>Studio: {{ anime.studio }}</p>
          <p>Genre: {{ anime.genre }}</p>
          <p>Rating: {{ anime.rating }}</p>
          <!-- Add more fields as necessary -->
        </div>
      </div>
    </div>
    <div v-else>
      <p>No anime selected.</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AnimeCard',
    props: {
      anime: {
        type: Object,
        required: false
      }
    }
  };
  </script>
  
  <style scoped>
  .anime-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 16px;
    max-width: 300px;
    background-color: #f9f9f9;
  }
  
  .anime-image {
    width: 100%;
    height: auto;
    border-radius: 4px;
  }
  
  .anime-info {
    text-align: center;
    margin-top: 12px;
  }
  </style>
  