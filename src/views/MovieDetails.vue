<script>
import axios from "axios";
export default {
  data() {
    return {
      movieId: this.$route.params.id,
      movieDetails: [],
      movieProviders: [],
      movieVote: null,
      IMG_URL: "https://image.tmdb.org/t/p/w500",
    };
  },
  mounted: function () {
    this.fetchMoveDetails();
  },
  methods: {
    async fetchMovieProviders() {
      const options = {
        method: "GET",
        url: ``,
        params: { language: "en-US" },
        headers: {
          accept: "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
      };

      try {
        const response = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.movieId}/watch/providers`,
          options
        );
        this.movieProviders = response.data;
        console.log(response.data);
      } catch (error) {
        console.error(error);
      }
    },
    async fetchMoveDetails() {
      const options = {
        method: "GET",
        url: ``,
        params: { language: "en-US" },
        headers: {
          accept: "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
      };

      try {
        const response = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.movieId}`,
          options
        );
        this.movieDetails = response.data;
        this.movieVote = response.data.vote_average * 10;
        console.log(response.data);
        console.log(this.movieVote);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
<template>
  <div class="hero min-h-screen bg-base-200">
    <div class="hero-content flex-col lg:flex-row">
      <img
        :src="`${IMG_URL}${movieDetails.poster_path}`"
        class="mr-6 max-w-sm rounded-lg shadow-2xl"
      />
      <div>
        <h1 class="text-5xl font-bold">{{ movieDetails.title }}</h1>
        <div class="flex flex-row">
          <div v-for="genre in movieDetails.genres">
            <div class="mr-2 text-sm text-gray-500">
              <span>
                {{ genre.name }}
              </span>
            </div>
          </div>
        </div>
        <p class="py-6 text-slate-400">{{ movieDetails.tagline }}</p>
        <h1 class="font-bold text-2xl">Overview</h1>
        <p class="py-2">{{ movieDetails.overview }}</p>
        <div
          class="radial-progress"
          :style="'--value:' + Math.floor(movieVote)"
        >
          {{ Math.floor(movieVote) }}%
        </div>
      </div>
    </div>
  </div>
</template>
