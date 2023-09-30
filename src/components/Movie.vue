<script>
export default {
  props: {
    name: String,
    img_path: String,
    release_date: String,
    vote_average: Number,
  },
  data() {
    return {
      IMG_URL: "https://image.tmdb.org/t/p/w500" + this.img_path,
      color: "",
    };
  },
  mounted: function () {
    if (this.vote_average >= 7) {
      this.color = "rgb(34, 197, 94)";
    }
    if (this.vote_average > 5 && this.vote_average < 7) {
      this.color = "rgb(249, 115, 22)";
    }
    if (this.vote_average > 0 && this.vote_average < 5) {
      this.color = "rgb(220, 38, 38)";
    }
  },
};
</script>
<template>
  <div class="flex flex-col w-40 m-10">
    <img class="rounded-lg w-40" :src="IMG_URL" alt="POSTER" />
    <div class="flex flex-row mt-2">
      <div class="flex flex-initial flex-col w-64">
        <span class="font-bold">{{ name }}</span>
        <span class="text-sm text-gray-500">{{ release_date }}</span>
      </div>
      <div class="flex h-fit text-center justify-center">
        <span
          v-bind:style="{ backgroundColor: color }"
          class="text-decoration-none rounded-full p-2 text-sm text-white font-bold"
          >{{ vote_average }}</span
        >
      </div>
    </div>
  </div>
</template>
