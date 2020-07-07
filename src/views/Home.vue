<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Add a new movie</h2>
    <ul>
      <li v-for="error in errors">{{error}}</li>
    </ul>

    <div>
      Title: <input type="text" v-model="newMovieTitle"> <br>
      Year: <input type="text" v-model="newMovieYear"> <br>
      Plot: <input type="text" v-model="newMoviePlot"> <br>
      Director: <input type="text" v-model="newMovieDirector"> <br>
      English: <input type="text" v-model="newMovieEnglish"> <br>
      <button v-on:click="createMovie()">Create Movie</button>
    </div>

    <div v-for="movie in movies">
      <h2> {{movie.title}}</h2>
      <p> {{movie.year}} </p>
      <p> {{movie.plot}} </p>
      <p> {{movie.director}} </p>
      <button v-on:click="showInfo(movie)">Show more info</button>
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h2>Movie Details</h2>
        <p>Title: <input type="text" v-model="currentMovie.title"> </p>
        <p>Year: <input type="text" v-model="currentMovie.year"> </p>
        <p>Plot: <input type="text" v-model="currentMovie.plot"> </p>
        <p>Director: <input type="text" v-model="currentMovie.director"> </p>
        <p>English: <input type="text" v-model="currentMovie.english"> </p>
        <div>
          <button v-on:click="updateMovie(currentMovie)">Update</button>
          <button v-on:click="destroyMovie(currentMovie)">Delete</button>
          <button>Close</button>
        </div>
      </form>
    </dialog>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Your Movies!",
      movies: [],
      errors: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
      newMovieEnglish: "",
      currentMovie: {}
    };
  },
  created: function() {
    this.indexMovies();
  },
  methods: {
    indexMovies: function() {
      axios.get("/api/movies").then(response => {
        console.log("All movies:", response.data);
        this.movies = response.data;
      });
    },
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector,
        english: this.newMovieEnglish
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          console.log("New Movie:", response.data);
          this.movies.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showInfo: function(movie) {
      this.currentMovie = movie;
      console.log(movie);
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function(movie) {
      var params = {
        title: movie.title,
        year: movie.year,
        plot: movie.plot,
        director: movie.director,
        english: movie.english
      };
      axios.patch(`/api/movies/${movie.id}`, params).then(response => {
        console.log("Updated:", response.data);
      });
    },
    destroyMovie: function(movie) {
      axios.delete(`/api/movies/${movie.id}`).then(response => {
        console.log("Deleted:", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>
