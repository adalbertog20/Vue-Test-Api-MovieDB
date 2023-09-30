<script>
import { RouterLink, RouterView } from "vue-router";
export default {
  data() {
    return {
      is_loged: localStorage.getItem("is_loged"),
      session_id: localStorage.getItem("session_id"),
    };
  },
  mounted: function () {
    this.$forceUpdate();
  },
  methods: {
    logout() {
      console.log("logout");
      localStorage.clear();
      this.$forceUpdate();
    },
  },
};
</script>
<template>
  <div class="navbar bg-base-100">
    <div class="flex-1">
      <RouterLink class="btn btn-ghost normal-case text-xl" to="/"
        >CT Movies</RouterLink
      >
    </div>
    <div class="flex-none gap-2">
      <div class="form-control">
        <input
          type="text"
          placeholder="Search"
          class="input input-bordered w-24 md:w-auto"
        />
      </div>
      <div v-if="!is_loged">
        <RouterLink to="/login">Login</RouterLink>
      </div>
      <div v-if="is_loged" class="dropdown dropdown-end">
        <label tabindex="0" class="btn btn-ghost btn-circle avatar">
          <div class="w-10 rounded-full">
            <img src="../assets/gamer.png" class="text-white" />
          </div>
        </label>
        <ul
          tabindex="0"
          class="mt-3 z-[1] p-2 shadow menu menu-sm dropdown-content bg-base-100 rounded-box w-52"
        >
          <li>
            <a class="justify-between">
              Profile
              <span class="badge">New</span>
            </a>
          </li>
          <li><a>Settings</a></li>
          <li>
            <button @click="logout" class="btn btn-ghost btn-sm">Logout</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
