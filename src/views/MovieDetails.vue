<script>
import axios from "axios";
import { getCurrentInstance } from "vue";
import { RouterLink } from "vue-router";
export default {
  data() {
    return {
      movieId: this.$route.params.id,
      movieDetails: [],
      movieProviders: [],
      movieVote: null,
      ratedMovies: [],
      value: 5,
      color: "",
      rating: 0,
      isRated: false,
      is_loged: localStorage.getItem("is_loged"),
      session_id: localStorage.getItem("session_id"),
      IMG_URL: "https://image.tmdb.org/t/p/w500",
    };
  },
  mounted: function () {
    this.fetchMoveDetails();
    setTimeout(() => {
      if (this.movieVote >= 70) {
        this.color = "text-green-400";
      }
      if (this.movieVote > 50 && this.movieVote < 70) {
        this.color = "text-orange-400";
      }
      if (this.movieVote < 50) {
        this.color = "text-red-400";
      }
    }, 1000);
    this.fetchRatedMovies();
    setTimeout(() => {
      console.log(this.ratedMovies);
      this.ratedMovies.forEach((r) => {
        if (r.id == this.movieId) {
          this.isRated = true;
          this.rating = r.rating;
        }
      });
    }, 400);
  },
  methods: {
    changeRoute(route) {
      this.$router.push(route);
    },
    deleteRating() {
      const options = {
        method: 'DELETE',
        headers: {
          accept: 'application/json', 'Content-Type': 'application/json;charset=utf-8',
          Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY'
        },

      };

      fetch(`https://api.themoviedb.org/3/movie/${this.movieId}/rating?session_id=${this.session_id}`, options)
        .then(response => response.json())
        .then(response => console.log(response))
        .catch(err => console.error(err));
      this.changeRoute({ name: 'home' });
    },
    fetchRatedMovies() {
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
      };
      fetch(
        `https://api.themoviedb.org/3/account/${this.session_id}/rated/movies?language=en-US&page=1&session_id=${this.session_id}&sort_by=created_at.asc`,
        options
      )
        .then((response) => response.json())
        .then((response) => (this.ratedMovies = response.results))
        .catch((err) => console.error(err));
    },
    sendRating() {
      console.log(this.value);
      const options = {
        method: "POST",
        headers: {
          accept: "application/json",
          "Content-Type": "application/json;charset=utf-8",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
        body: `{"value":${this.value}}`,
      };
      fetch(
        `https://api.themoviedb.org/3/movie/${this.movieId}/rating?session_id=${this.session_id}`,
        options
      )
        .then((response) => response.json())
        .then((response) => console.log(response))
        .catch((err) => console.error(err));
      this.$router.push({ name: "home", id: this.movieId });
    },
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
      } catch (error) {
        console.error(error);
      }
    },
  },
  components: { RouterLink },
};
</script>
<template>
  <div class="hero min-h-screen" :style="{ backgroundImage: `url(${IMG_URL}${movieDetails.backdrop_path})` }">
    <div class="hero-overlay bg-opacity-80"></div>
    <div class="hero-content">
      <div class="hero-content flex-col lg:flex-row">
        <img :src="`${IMG_URL}${movieDetails.poster_path}`" class="mr-6 max-w-sm rounded-lg shadow-2xl" />
        <div>
          <div class="flex flex-row items-center">
            <h1 class="text-5xl font-bold">{{ movieDetails.title }}</h1>
            <button v-if="is_loged" onclick="my_modal_2.showModal()" class="btn btn-sm ml-10 mt-2.5" :disabled="isRated">
              Add Rating
            </button>

            <div v-if="isRated" class="ml-10">
              <span class="text-2xl mr-10">My Rating: {{ rating }}</span>
              <button @click="deleteRating" class="btn btn-sm btn-square">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                  stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
            </div>
            <!-- Moda For Rating Movies -->
            <dialog id="my_modal_2" class="modal">
              <div class="modal-box">
                <h3 class="font-bold text-lg">Add Rating!</h3>
                <input class="input input-bordered mt-2" type="number" disabled :value="value" />
                <input v-model="value" type="range" min="0" max="10" step="0.5" class="range range-primary mt-4" />
                <form method="dialog">
                  <button @click="sendRating" class="btn btn-outline b btn-ghost w-full mt-4">
                    Rate
                  </button>
                </form>
              </div>
              <form method="dialog" class="modal-backdrop">
                <button>close</button>
              </form>
            </dialog>
            <!-- End OF Modal -->
          </div>
          <div class="flex flex-row">
            <div v-for="genre in movieDetails.genres">
              <div class="mr-2 text-sm text-gray-400">
                <span>
                  {{ genre.name }}
                </span>
              </div>
            </div>
          </div>
          <p class="py-6 text-slate-400">{{ movieDetails.tagline }}</p>
          <h1 class="font-bold text-2xl">Overview</h1>
          <p class="py-2">{{ movieDetails.overview }}</p>
          <div :class="'mt-20 radial-progress ' + color" :style="'--size:8rem; --value:' + Math.floor(movieVote)">
            {{ Math.floor(movieVote) }}%
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
