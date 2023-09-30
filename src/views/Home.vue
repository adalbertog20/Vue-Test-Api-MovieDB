<script>
import axios from "axios";
import Movie from "../components/Movie.vue";
export default {
  components: {
    Movie,
  },
  data() {
    return {
      is_loged: localStorage.getItem("is_loged"),
      movies: [],
    };
  },
  mounted: function () {
    this.fetchMovies();
  },
  methods: {
    changeRoute(route) {
      this.$router.push(route);
    },
    async fetchMovies() {
      const options = {
        method: "GET",
        url: "https://api.themoviedb.org/3/movie/popular",
        params: { language: "en-US", page: "1", region: "US" },
        headers: {
          accept: "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
      };
      try {
        const response = await axios.get(
          "https://api.themoviedb.org/3/movie/popular"
        );
        this.movies = response.data.results;
        console.log(response.data.results);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
<template>
  <h2 class="text-center font-custom">Most Popular Movies</h2>
  <div class="flex items-center justify-center">
    <div class="grid grid-cols-4" v-if="is_loged">
      <div class="font-custom" v-for="movie in movies" :key="movie.id">
        <Movie
          @click="
            changeRoute({ name: 'movieDetails', params: { id: movie.id } })
          "
          :vote_average="movie.vote_average"
          :release_date="movie.release_date"
          :name="movie.title"
          :img_path="movie.poster_path"
        />
      </div>
    </div>
  </div>
</template>
