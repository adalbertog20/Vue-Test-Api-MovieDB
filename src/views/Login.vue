<script>
import axios from "axios";
import { ref } from "vue";
export default {
  props: {
    rt: String,
  },
  data() {
    return {
      promise_session: "",
      session_id: ref(""),
      promise_token: "",
      request_token: "",
      promise_valid_request_token: "",
      valid_request_token: ref(""),
      username: "",
      password: "",
      is_loged: localStorage.getItem("is_loged"),
      error: ref(false),
    };
  },
  mounted: function () {
    this.fetchRequestToken();
  },
  methods: {
    login() {
      console.log(this.request_token);
      this.fetchValidRequestToken(this.request_token);
      setTimeout(() => {
        console.log(this.promise_valid_request_token);
        this.promise_valid_request_token
          .then((value) => {
            this.valid_request_token = value.request_token;
          })
          .catch((error) => {
            console.log(this.error);
          });
      }, 1000);

      setTimeout(() => {
        this.fetchCreateSession();
      }, 1000);
    },
    async fetchCreateSession() {
      const options = {
        method: "POST",
        url: "https://api.themoviedb.org/3/authentication/session/new",
        headers: {
          accept: "application/json",
          "content-type": "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
        data: { request_token: `${this.valid_request_token}` },
      };
      axios
        .request(options)
        .then(function (response) {
          localStorage.setItem("session_id", response.data.session_id);
          localStorage.setItem("is_loged", response.data.success);
          console.log(response.data.session_id);
        })
        .catch(function (error) {
          console.error(error);
        });
    },
    async fetchValidRequestToken(rt) {
      const options = {
        method: "POST",
        data: {
          username: `${this.username}`,
          password: `${this.password}`,
          request_token: `${rt}`,
        },
      };
      try {
        const response = await fetch(
          "https://api.themoviedb.org/3/authentication/token/validate_with_login",
          {
            method: "POST",
            headers: {
              Accept: "application/json",
              "Content-type": "application/json",
              Authorization:
                "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
            },
            body: JSON.stringify({
              username: `${this.username}`,
              password: `${this.password}`,
              request_token: `${rt}`,
            }),
          }
        );
        this.promise_valid_request_token = response.json();
      } catch (error) {
        this.error = ref(false);
        console.error(error);
      }
    },
    async fetchRequestToken() {
      const options = {
        method: "GET",
        headers: {
          accept: "application/json",
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1OTBkMzEyZDM1YjUzMzQyZTU5MmM1OTYzNWQ5YjU5ZSIsInN1YiI6IjY1MTQ0ZDAyYTE5OWE2MDBjNDljMWJmMiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.QSQFrA3vcF_XRV6gerBwXmooqC1rzktBYLNkIjY8VDY",
        },
      };
      try {
        const response = await axios.get(
          "https://api.themoviedb.org/3/authentication/token/new",
          options
        );
        this.request_token = response.data.request_token;
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
<template>
  <div class="flex items-center justify-center h-screen">
    <div class="flex flex-col" v-if="!is_loged">
      <form>
        <div class="mb-6">
          <label class="label">
            <span class="label-text">Username</span>
          </label>
          <input
            class="input input-bordered input-primary w-full max-w-xs"
            v-model="username"
            type="text"
            placeholder="type your username"
            required
          />
        </div>
        <div class="mb-6">
          <label class="label">
            <span class="label-text">Password</span>
          </label>
          <input
            class="input input-bordered input-primary w-full max-w-xs"
            v-model="password"
            type="password"
            placeholder="type your password"
            required
          />
        </div>
        <div class="w-full">
          <RouterLink to="/">
            <button @click="login" type="button" class="w-full btn btn-primary">
              Login
            </button>
          </RouterLink>
        </div>
      </form>
    </div>
  </div>
</template>
