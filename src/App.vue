<template>
  <div id="app">
    <SearchBar @onSearch="onSearch"></SearchBar>
    <h2>Movies</h2>
    <MediaCard
      v-for="movie in moviesList"
      :key="movie.id"
      :title="movie.title"
      :countryCode="movie.original_language"
      :originalTitle="movie.original_title"
      :rank="movie.vote_average"
    ></MediaCard>
    <h2>TV Series</h2>
    <MediaCard
      v-for="serie in seriesList"
      :key="serie.id"
      :title="serie.name"
      :countryCode="serie.original_language"
      :originalTitle="serie.original_name"
      :rank="serie.vote_average"
    ></MediaCard>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import MediaCard from "./components/MediaCard.vue";
export default {
  name: "App",
  components: { SearchBar, MediaCard },
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
    };
  },
  methods: {
    onSearch(queryString) {
      this.search("movies", queryString);
      this.search("series", queryString);
    },
    search(apiType, queryText) {
      axios
        .get(this.apiEndpoint + this.apiConfig[apiType].url, {
          params: { api_key: this.apiKey, query: queryText },
        })
        .then((resp) => {
          console.log(resp.data);
          this[this.apiConfig[apiType].variableListName] = resp.data.results;
        });
    },

    // searchSeries(searchedText) {
    //   axios
    //     .get(this.apiEndpoint + "/search/tv", {
    //       params: { api_key: this.apiKey, query: searchedText },
    //     })
    //     .then((resp) => console.log(resp.data));
    // },
  },
  mounted() {},
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
