<template>
  <div v-if="!isFetchingData">
    <Shell
      :genreList="moviesGenreList"
      :isMovie="true"
      :mediaItemsList="moviesList"
      shellTitle="Movies"
    ></Shell>
    <Shell
      :genreList="seriesGenreList"
      :isMovie="false"
      :mediaItemsList="seriesList"
      shellTitle="Series"
    ></Shell>
  </div>
</template>

<script>
import axios from "axios";
import Shell from "./Shell.vue";

export default {
  components: { Shell },
  name: "SearchPage",
  props: {
    queryString: String,
    moviesGenreList: Array,
    seriesGenreList: Array,
  },
  data() {
    return {
      isFetchingData: false,
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
        this.isFetchingData = true;
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
        this.isFetchingData = false;
        this.$emit("loadingComplete");
      }
    },
  },
  computed: {},
  mounted() {
    this.onSearch();
  },
};
</script>

<style>
</style>