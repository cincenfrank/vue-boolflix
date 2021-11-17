<template>
  <div id="app">
    <SearchBar @onSearch="onSearch"></SearchBar>
    <h2>Movies</h2>
    <ul>
      <li v-for="movie in moviesList" :key="movie.id">
        {{ movie.title }}
        <ul>
          <li>{{ movie.original_title }}</li>
          <li>{{ movie.original_language }}</li>
          <li>{{ movie.popularity }}</li>
          <!-- <li>{{ movie.original_title }}</li> -->
        </ul>
      </li>
    </ul>
    <h2>TV Series</h2>
    <ul>
      <li v-for="movie in seriesList" :key="movie.id">
        {{ movie.name }}
        <ul>
          <li>{{ movie.original_name }}</li>
          <li>{{ movie.original_language }}</li>
          <li>{{ movie.popularity }}</li>
          <!-- <li>{{ movie.original_title }}</li> -->
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
export default {
  name: "App",
  components: { SearchBar },
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
        .then(
          (resp) =>
            (this[this.apiConfig[apiType].variableListName] = resp.data.results)
        );
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
