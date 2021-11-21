<template>
  <div v-if="!isFetchingData">
    <Shell
      :filterList="moviesFilterList"
      :isMovie="true"
      :mediaItemsList="moviesList"
      shellTitle="Movies"
    ></Shell>
    <Shell
      :filterList="seriesFilterList"
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
          genreVariableListName: "moviesGenreList",
          genreFilterList: "moviesFilterList",
        },
        series: {
          url: "/search/tv",
          variableListName: "seriesList",
          genreVariableListName: "seriesGenreList",
          genreFilterList: "seriesFilterList",
        },
      },

      moviesList: [],
      seriesList: [],
      moviesFilterList: [],
      seriesFilterList: [],
      pendingCalls: 0,
    };
  },

  methods: {
    onSearch() {
      if (this.queryString.trim()) {
        this.isFetchingData = true;
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
          this[this.apiConfig[apiType].variableListName]["genre_names"] = [];
          this[this.apiConfig[apiType].variableListName].forEach((list) => {
            list.genre_ids.forEach((el) => {
              const name = this.getGenreName(el, apiType);
              if (name) {
                if (!list.genre_names) {
                  list.genre_names = [];
                }
                list.genre_names.push(name);
                if (
                  !this[this.apiConfig[apiType].genreFilterList].includes(name)
                ) {
                  this[this.apiConfig[apiType].genreFilterList].push(name);
                }
              }
            });
          });

          this.pendingCalls--;
        });
    },
    getGenreName(genreId, apiType) {
      for (const index in this[this.apiConfig[apiType].genreVariableListName]) {
        const genreRef =
          this[this.apiConfig[apiType].genreVariableListName][index];
        if (genreRef.id === genreId) {
          return genreRef.name;
        }
      }
      return "not-available";
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