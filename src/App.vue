<script>
import { RouterLink, RouterView } from 'vue-router'
import PaginatedList from './components/PaginatedList.vue';
import AnimeCard from './components/AnimeCard.vue'

async function getData() {
  try {
    const response = await fetch("https://api.jikan.moe/v4/anime");
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}
async function main() {
  const data = await getData();
}

main();
export default {
  name: 'App',
  components: {
    PaginatedList, AnimeCard
  },methods: {
    handlePageDataUpdated(newData) {
      // Handle the new page data received from the child component
      console.log('Received new page data:', newData);
      // You can process or store the new data as needed
      this.animeData = newData.data;
    }
  },
  data() {
  return {
    animeData: []
  };
}
}
</script>

<template>
  
  <PaginatedList @page-data-updated="handlePageDataUpdated"/>
  <div v-if="animeData.length">
    <AnimeCard v-for="(anime, index) in animeData" :key="index" :anime="anime" />
  </div>
  
  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
