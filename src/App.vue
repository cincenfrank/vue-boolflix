<template>
  <div id="app" class="vh-100 overflow-hidden d-flex flex-column">
    <header>
      <nav>
        <img src="@/assets/logo_boolflix.png" alt="logo boolflix" />
        <SearchBar @onSearch="onSearch"></SearchBar>
      </nav>
    </header>
    <main
      class="overflow-hidden bg-dark flex-grow-1 py-5 position-relative h-100"
    >
      <div class="h-100 overflow-auto">
        <SearchPage
          class="container"
          v-if="routingObject.searchPage"
          @loadingComplete="onLoadingComplete('searchPage')"
          :queryString="queryString"
        ></SearchPage>
        <LoadingPage
          class="
            container
            position-absolute
            top-0
            bottom-0
            bg-dark
            start-0
            end-0
          "
          v-if="routingObject.loadingPage"
        ></LoadingPage>
      </div>
    </main>
  </div>
</template>

<script>
import LoadingPage from "./components/LoadingPage.vue";
import SearchBar from "./components/SearchBar.vue";
import SearchPage from "./components/SearchPage.vue";
// import MediaCard from "./components/MediaCard.vue";
export default {
  name: "App",
  components: { SearchBar, SearchPage, LoadingPage },
  data() {
    return {
      queryString: "",

      routingObject: {
        searchPage: false,
        loadingPage: true,
      },
    };
  },
  methods: {
    onSearch(newQueryString) {
      if (newQueryString.trim()) {
        // debugger;
        this.queryString = newQueryString;
        this.routingObject.searchPage = true;
        // this.searchPage = true;
      }
      // this.search("movies", queryString);
      // this.search("series", queryString);
    },
    onLoadingComplete() {
      this.routingObject.loadingPage = false;
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
