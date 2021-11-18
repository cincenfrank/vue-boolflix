<template>
  <div>
    <h2 v-if="moviesList.length > 0" class="text-white">Movies</h2>
    <div class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5">
      <div class="col p-0" v-for="movie in moviesList" :key="movie.id">
        <MediaCard
          :title="movie.title"
          :countryCode="movie.original_language"
          :originalTitle="movie.original_title"
          :rank="movie.vote_average"
          :posterPath="movie.poster_path"
          :overview="movie.overview"
        ></MediaCard>
      </div>
    </div>

    <h2 v-if="seriesList.length > 0" class="text-white">TV Series</h2>
    <div class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5">
      <div class="col p-0" v-for="serie in seriesList" :key="serie.id">
        <MediaCard
          :title="serie.name"
          :countryCode="serie.original_language"
          :originalTitle="serie.original_name"
          :rank="serie.vote_average"
          :posterPath="serie.poster_path"
          :overview="serie.overview"
        ></MediaCard>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MediaCard from "./MediaCard.vue";

export default {
  components: { MediaCard },
  name: "SearchPage",
  props: { queryString: String },
  data() {
    return {
      apiKey: "4315a46b8f5cb187030ac497d365dea0",
      apiEndpoint: "https://api.themoviedb.org/3",
      apiConfig: {
        movies: {
          url: "/search/movie",
          variableListName: "moviesList",
        },
        series: {
          url: "/search/tv",
          variableListName: "seriesList",
        },
      },
      moviesList: [],
      seriesList: [],
      pendingCalls: 0,
    };
  },

  methods: {
    onSearch() {
      if (this.queryString.trim()) {
        // debugger;
        this.search("movies", this.queryString);
        this.search("series", this.queryString);
      }
    },
    search(apiType, queryText) {
      this.pendingCalls++;
      axios
        .get(this.apiEndpoint + this.apiConfig[apiType].url, {
          params: { api_key: this.apiKey, query: queryText },
        })
        .then((resp) => {
          console.log(resp.data);
          this[this.apiConfig[apiType].variableListName] = resp.data.results;
          this.pendingCalls--;
        });
    },
  },
  watch: {
    queryString: function () {
      this.onSearch();
    },
    pendingCalls: function () {
      if (this.pendingCalls === 0) {
        this.$emit("loadingComplete");
      }
    },
  },
  mounted() {
    this.onSearch();
  },
};
</script>

<style>
</style>