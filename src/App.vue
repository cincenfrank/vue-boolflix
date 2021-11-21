<template>
  <div id="app" class="vh-100 overflow-hidden d-flex flex-column">
    <Header @onSearch="onSearch"></Header>
    <!-- <header>
      <nav>
        <img src="@/assets/logo_boolflix.png" alt="logo boolflix" />
        <SearchBar @onSearch="onSearch"></SearchBar>
      </nav>
    </header> -->
    <main class="overflow-hidden bg-dark flex-grow-1 position-relative h-100">
      <div class="h-100 overflow-auto">
        <SearchPage
          class="container"
          v-if="routingObject.searchPage"
          @loadingComplete="onLoadingComplete('searchPage')"
          :queryString="queryString"
          :moviesGenreList="moviesGenreList"
          :seriesGenreList="seriesGenreList"
        ></SearchPage>
        <HomePage
          class="
            container
            position-absolute
            top-0
            bottom-0
            bg-dark
            start-0
            end-0
          "
          v-if="routingObject.homePage"
        ></HomePage>
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
import HomePage from "./components/HomePage.vue";
import LoadingPage from "./components/LoadingPage.vue";
import SearchPage from "./components/SearchPage.vue";
import axios from "axios";
import Header from "./components/Header.vue";
export default {
  name: "App",
  components: { SearchPage, LoadingPage, HomePage, Header },
  data() {
    return {
      queryString: "",

      routingObject: {
        searchPage: false,
        loadingPage: true,
        homePage: false,
      },
      apiGenreConfig: {
        seriesGenre: {
          url: "/genre/tv/list",
          variableListName: "seriesGenreList",
        },
        moviesGenre: {
          url: "/genre/movie/list",
          variableListName: "moviesGenreList",
        },
      },
      moviesGenreList: [],
      seriesGenreList: [],
      apiKey: "4315a46b8f5cb187030ac497d365dea0",
      apiEndpoint: "https://api.themoviedb.org/3",
      isFetchingData: false,
      pendingCalls: 0,
    };
  },
  watch: {
    pendingCalls: function () {
      if (
        this.pendingCalls === 0 &&
        this.moviesGenreList.length > 0 &&
        this.seriesGenreList.length > 0
      ) {
        setTimeout(() => {
          this.initData();
        }, 2000);
      }
    },
  },
  methods: {
    initData() {
      this.resetRoutingPages();

      this.routingObject.loadingPage = false;
      this.routingObject.homePage = true;
    },
    resetRoutingPages() {
      for (const key in this.routingObject) {
        if (key !== "loadingPage") {
          this.routingObject[key] = false;
        }
      }
    },
    onSearch(newQueryString) {
      if (this.queryString !== newQueryString) {
        if (newQueryString.trim()) {
          this.routingObject.loadingPage = true;
          this.resetRoutingPages();

          this.queryString = newQueryString;
          this.routingObject.searchPage = true;
        }
      }
      // this.search("movies", queryString);
      // this.search("series", queryString);
    },

    onLoadingComplete() {
      this.routingObject.loadingPage = false;
    },
    getGenres() {
      for (const el in this.apiGenreConfig) {
        // debugger;
        this.pendingCalls++;
        axios
          .get(this.apiEndpoint + this.apiGenreConfig[el].url, {
            params: { api_key: this.apiKey },
          })
          .then((resp) => {
            // debugger;
            this[this.apiGenreConfig[el].variableListName] = resp.data.genres;
            this.pendingCalls--;
          });
      }
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
  mounted() {
    this.getGenres();
  },
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
