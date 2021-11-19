<template>
  <div>
    <h2 v-if="!isFetchingData" class="text-white">Movies</h2>
    <p v-if="moviesList.length === 0" class="text-white">
      No TV Serie Found for your search
    </p>
    <div
      v-else-if="moviesList.length > 0"
      class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5"
    >
      <div
        class="col p-0"
        v-for="movie in moviesList"
        :key="'movie-' + movie.id"
      >
        <MediaCard
          :id="movie.id"
          type="movie"
          :title="movie.title"
          :countryCode="movie.original_language"
          :originalTitle="movie.original_title"
          :rank="movie.vote_average"
          :posterPath="movie.poster_path"
          :overview="movie.overview"
        ></MediaCard>
      </div>
    </div>

    <h2 v-if="!isFetchingData" class="text-white">TV Series</h2>
    <p v-if="seriesList.length === 0" class="text-white">
      No TV Serie Found for your search
    </p>
    <div
      v-else-if="seriesList.length > 0"
      class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5"
    >
      <div class="col p-0" v-for="serie in seriesList" :key="'tv-' + serie.id">
        <MediaCard
          :id="serie.id"
          type="tv"
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
      isFetchingData: false,
      apiKey: "4315a46b8f5cb187030ac497d365dea0",
      apiEndpoint: "https://api.themoviedb.org/3",
      apiConfig: {
        movies: {
          url: "/search/movie",
          variableListName: "moviesList",
          // creditsSuffix: "/movie/",
        },
        series: {
          url: "/search/tv",
          variableListName: "seriesList",
          // creditsSuffix: "/tv/",
        },
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
      moviesList: [],
      seriesList: [],
      moviesGenreList: [],
      seriesGenreList: [],
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
    search(apiType, queryText) {
      this.pendingCalls++;
      axios
        .get(this.apiEndpoint + this.apiConfig[apiType].url, {
          params: { api_key: this.apiKey, query: queryText },
        })
        .then((resp) => {
          console.log(resp.data);
          this[this.apiConfig[apiType].variableListName] = resp.data.results;

          // this[this.apiConfig[apiType].variableListName].forEach((element) => {
          //   // element.cast = [];
          //   this.getCredits(apiType, element.id, element);
          // });
          this.pendingCalls--;
        });
    },
    // getCredits(apiType, id, objectReference) {
    //   this.pendingCalls++;
    //   axios
    //     .get(
    //       this.apiEndpoint +
    //         this.apiConfig[apiType].creditsSuffix +
    //         id +
    //         "/credits",
    //       {
    //         params: { api_key: this.apiKey },
    //       }
    //     )
    //     .then((resp) => {
    //       // debugger;
    //       // console.log(resp.data.cast);

    //       objectReference["cast"] = [...resp.data.cast];
    //       this.pendingCalls--;
    //     });
    // },
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
  mounted() {
    this.getGenres();
    this.onSearch();
  },
};
</script>

<style>
</style>