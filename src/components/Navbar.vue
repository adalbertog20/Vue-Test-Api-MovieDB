<script>
import { RouterLink, RouterView } from "vue-router";
export default {
  data() {
    return {
      is_loged: localStorage.getItem("is_loged"),
    };
  },
};
</script>
<template>
  <nav class="bg-black border-gray-200">
    <div
      class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4"
    >
      <a href="https://flowbite.com/" class="flex items-center">
        <img src="../assets/ct.png" class="h-12 mr-3" alt="Flowbite Logo" />
      </a>
      <div class="hidden w-full md:block md:w-auto" id="navbar-default">
        <ul class="list-none flex flex-row">
          <li class="mr-2">
            <RouterLink
              class="hover:text-red-500 active:text-red-300 no-underline text-white"
              to="/"
              >Home</RouterLink
            >
          </li>
          <li v-if="!is_loged">
            <RouterLink class="no-underline text-white" to="/login"
              >Login</RouterLink
            >
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>
