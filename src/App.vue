<script>
import { RouterLink, RouterView } from "vue-router";
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
export default {
  components: {
    Navbar,
    Footer,
  },
  data() {
    return {
      is_loged: localStorage.getItem("is_loged"),
    };
  },
};
</script>

<template>
  <Navbar />
  <RouterView></RouterView>
</template>
