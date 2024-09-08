<template>
    <div>
      <div v-if="loading">Loading...</div>
      <div v-if="error">{{ error }}</div>
      <ul v-if="data && data.results">
        <li v-for="item in data.results" :key="item.id">{{ item.title }}</li>
      </ul>
      <button @click="prevPage" :disabled="currentPage <= 1">Previous</button>
      <button @click="nextPage" :disabled="!hasMorePages">Next</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        data: null,
        currentPage: 1,
        pageSize: 10,
        loading: false,
        error: null,
        hasMorePages: false,
      };
    },
    methods: {
      async fetchData(page = 1) {
        this.loading = true;
        this.error = null;
        try {
          const response = await fetch(`https://api.jikan.moe/v4/anime?page=${page}&page_size=${this.pageSize}`);
          if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
          }
          const result = await response.json();
          this.data = result;
          // Check if there are more pages based on your API's response structure
          this.hasMorePages = result.pagination && result.pagination.has_next_page;
          this.$emit('page-data-updated', result);
        } catch (err) {
          this.error = 'Error fetching data: ' + err.message;
        } finally {
          this.loading = false;
        }
      },
      nextPage() {
        if (this.hasMorePages) {
          this.currentPage++;
          this.fetchData(this.currentPage);
        }
      },
      prevPage() {
        if (this.currentPage > 1) {
          this.currentPage--;
          this.fetchData(this.currentPage);
        }
      },
    },
    mounted() {
      this.fetchData();
    },
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  