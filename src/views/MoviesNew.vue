<template>
  <div class="movies-new">
    
    <form v-on:submit.prevent="createMovie()">
      <h1>New Movie</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title">
      </div>
      <div class="form-group">
        <label>Year:</label>
        <input type="text" class="form-control" v-model="year">
      </div>
      <div class="form-group">
        <label>Plot:</label>
        <input type="text" class="form-control" v-model="plot">
        <small v-if="plot.length <= 250">Characters remaining: {{250 - plot.length}}</small>
        <small v-if="plot.length > 250" class="text-danger">Character count: {{plot.length}} - Please limit length to 250 characters.</small>
      </div>
      <div class="form-group">
        <label>Director:</label>
        <input type="text" class="form-control" v-model="director">
      </div>
      <div class="form-group">
        <label>English:</label>
        <input type="text" class="form-control" v-model="english">
      </div>
      <input type="submit" class="btn btn-primary" value="Create">
    </form>

  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      errors: [],
      title: "",
      year: "",
      plot: "",
      director: "",
      english: ""
    };
  },
  created: function() {},
  methods: {
    createMovie: function() {
      var params = {
        title: this.title,
        year: this.year,
        plot: this.plot,
        director: this.director,
        english: this.english
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          this.$router.push(`/movies/${response.data.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>