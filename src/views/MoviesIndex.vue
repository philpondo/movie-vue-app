<template>
  <div class="movies-index">
    <div>Search by name: <input v-model="titleFilter" list="titles"></div>
      <datalist id="titles">
        <option v-for="movie in movies">{{ movie.title }}</option>
      </datalist>
    <div>
      <button>Sort Alphabetically</button>
      <div v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), 'title')">
        <h2>{{ movie.title }}</h2>
        <p>Year: {{ movie.year }}</p>
        <p>Plot: {{ movie.plot }}</p>
        <p>Director: {{ movie.director }}</p>
        <p>English: {{ movie.english }}</p>
        <router-link v-bind:to="`/movies/${movie.id}`">More Info</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      titleFilter: ""
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      console.log("All Movies:", response.data);
      this.movies = response.data;
    });
  }
};
</script>