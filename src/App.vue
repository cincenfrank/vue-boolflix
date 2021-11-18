<template>
  <div id="app" class="vh-100 overflow-hidden">
    <header>
      <nav>
        <span>BOOLFLIX</span>
        <SearchBar @onSearch="onSearch"></SearchBar>
      </nav>
    </header>
    <main class="overflow-auto h-100 bg-dark">
      <div class="container">
        <SearchPage v-if="searchPage" :queryString="queryString"></SearchPage>
        <!-- <h2 v-if="moviesList.length > 0">Movies</h2>
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
      <h2 v-if="seriesList.length > 0">TV Series</h2>
      <div class="shell">
        <MediaCard
          v-for="serie in seriesList"
          :key="serie.id"
          :title="serie.name"
          :countryCode="serie.original_language"
          :originalTitle="serie.original_name"
          :rank="serie.vote_average"
          :posterPath="serie.poster_path"
          :overview="serie.overview"
        ></MediaCard>
      </div> -->
      </div>
    </main>
  </div>
</template>

<script>
import SearchBar from "./components/SearchBar.vue";
import SearchPage from "./components/SearchPage.vue";
// import MediaCard from "./components/MediaCard.vue";
export default {
  name: "App",
  components: { SearchBar, SearchPage },
  data() {
    return { queryString: "", searchPage: false };
    //   return {
    //     apiKey: "4315a46b8f5cb187030ac497d365dea0",
    //     apiEndpoint: "https://api.themoviedb.org/3",
    //     apiConfig: {
    //       movies: {
    //         url: "/search/movie",
    //         variableListName: "moviesList",
    //       },
    //       series: {
    //         url: "/search/tv",
    //         variableListName: "seriesList",
    //       },
    //     },
    //     moviesList: [],
    //     seriesList: [],
    //   };
  },
  methods: {
    onSearch(newQueryString) {
      if (newQueryString.trim()) {
        // debugger;
        this.queryString = newQueryString;
        this.searchPage = true;
      }
      // this.search("movies", queryString);
      // this.search("series", queryString);
    },
    // search(apiType, queryText) {
    //   axios
    //     .get(this.apiEndpoint + this.apiConfig[apiType].url, {
    //       params: { api_key: this.apiKey, query: queryText },
    //     })
    //     .then((resp) => {
    //       console.log(resp.data);
    //       this[this.apiConfig[apiType].variableListName] = resp.data.results;
    //     });
    // },

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
@import "@/styles/app.scss";
// #app {
// font-family: Avenir, Helvetica, Arial, sans-serif;
// -webkit-font-smoothing: antialiased;
// -moz-osx-font-smoothing: grayscale;
// text-align: center;
// color: #2c3e50;
// margin-top: 60px;
// }
</style>
