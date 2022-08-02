<template>
  <div class="movie-title" style="color: #fff; font-size: larger">
    {{ movie.Title }}
  </div>
  <div class="movie-poster"><img :src="movie.Poster" alt="" /></div>
  <div class="movie-plot" style="color: #fff; font-size: larger">
    {{ movie.Plot }}
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "@/env";

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(
        `http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`
      )
        .then((response) => response.json())
        .then((data) => {
          movie.value = data;
        });
    });

    return { movie };
  },
};
</script>

<style>
.movie-detail {
  color: #fff;
}
</style>