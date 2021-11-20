<template>
  <div v-if="!isFetchingData">
    <div
      class="title-row d-flex justify-content-between py-3 align-items-center"
    >
      <h2 class="text-white">Movies</h2>
      <div>
        <label for="movieFilter" class="text-white pe-2"
          >Filter Visible Movies</label
        >
        <select name="movieFilter" id="movieFilter" v-model="movieFilterValue">
          <option
            v-for="filterValue in moviesFilterList"
            :key="filterValue + '-movie'"
            :value="filterValue"
          >
            {{ filterValue }}
          </option>
        </select>
        <div class="btn btn-danger" @click="movieFilterValue = ''">clear</div>
      </div>
    </div>
    <p v-if="displayMoviesList.length === 0" class="text-white">
      No Movies Found for your search
    </p>
    <div
      v-else-if="displayMoviesList.length > 0"
      class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5"
    >
      <div
        class="col p-0"
        v-for="movie in displayMoviesList"
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
          :genresList="movie.genre_names"
        ></MediaCard>
      </div>
    </div>
    <div
      class="title-row d-flex justify-content-between py-3 align-items-center"
    >
      <h2 class="text-white">TV Series</h2>
      <div>
        <label for="serieFilter" class="text-white pe-2"
          >Filter Visible TV Series</label
        >
        <select name="serieFilter" id="serieFilter" v-model="seriesFilterValue">
          <option
            v-for="filterValue in seriesFilterList"
            :key="filterValue + '-serie'"
            :value="filterValue"
          >
            {{ filterValue }}
          </option>
        </select>
        <div class="btn btn-danger" @click="seriesFilterValue = ''">clear</div>
      </div>
    </div>
    <p v-if="displaySeriesList.length === 0" class="text-white">
      No TV Serie Found for your search
    </p>
    <div
      v-else-if="displaySeriesList.length > 0"
      class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5"
    >
      <div
        class="col p-0"
        v-for="serie in displaySeriesList"
        :key="'tv-' + serie.id"
      >
        <MediaCard
          :id="serie.id"
          type="tv"
          :title="serie.name"
          :countryCode="serie.original_language"
          :originalTitle="serie.original_name"
          :rank="serie.vote_average"
          :posterPath="serie.poster_path"
          :overview="serie.overview"
          :genresList="serie.genre_names"
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
          // creditsSuffix: "/movie/",
        },
        series: {
          url: "/search/tv",
          variableListName: "seriesList",
          genreVariableListName: "seriesGenreList",
          genreFilterList: "seriesFilterList",

          // creditsSuffix: "/tv/",
        },
      },
      // apiGenreConfig: {
      //   seriesGenre: {
      //     url: "/genre/tv/list",
      //     variableListName: "seriesGenreList",
      //   },
      //   moviesGenre: {
      //     url: "/genre/movie/list",
      //     variableListName: "moviesGenreList",
      //   },
      // },
      moviesList: [],
      seriesList: [],
      moviesFilterList: [],
      seriesFilterList: [],
      movieFilterValue: "",
      seriesFilterValue: "",
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
    // getGenres() {
    //   for (const el in this.apiGenreConfig) {
    //     // debugger;
    //     this.pendingCalls++;
    //     axios
    //       .get(this.apiEndpoint + this.apiGenreConfig[el].url, {
    //         params: { api_key: this.apiKey },
    //       })
    //       .then((resp) => {
    //         // debugger;
    //         this[this.apiGenreConfig[el].variableListName] = resp.data.genres;
    //         this.pendingCalls--;
    //       });
    //   }
    // },
    search(apiType, queryText) {
      this.pendingCalls++;
      axios
        .get(this.apiEndpoint + this.apiConfig[apiType].url, {
          params: { api_key: this.apiKey, query: queryText },
        })
        .then((resp) => {
          console.log(resp.data);
          // debugger;
          // resp.data.results.genre_names = [];
          // resp.data.results.forEach((list) => {
          //   list.genre_ids.forEach((el) => {
          //     const name = this.getGenreName(el, apiType);
          //     if (name) {
          //       list.genre_names.push(name);
          //     }
          //   });
          // });
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
          // this[this.apiConfig[apiType].variableListName].forEach((element) => {
          //   // element.cast = [];
          //   this.getCredits(apiType, element.id, element);
          // });
          this.pendingCalls--;
        });
    },
    getGenreName(genreId, apiType) {
      // debugger;
      for (const index in this[this.apiConfig[apiType].genreVariableListName]) {
        const genreRef =
          this[this.apiConfig[apiType].genreVariableListName][index];
        // debugger;
        if (genreRef.id === genreId) {
          return genreRef.name;
        }
      }
      return "not-available";
      // return this[this.apiConfig[apiType].genreVariableListName].forEach(
      //   (element) => {}
      // );
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
  computed: {
    displayMoviesList() {
      if (this.movieFilterValue) {
        return this.moviesList.reduce((acc, current) => {
          // debugger;
          if (
            current.genre_names &&
            current.genre_names.includes(this.movieFilterValue)
          ) {
            acc.push(current);
          }
          return acc;
        }, []);
      }
      return this.moviesList;
    },
    displaySeriesList() {
      if (this.seriesFilterValue) {
        return this.seriesList.reduce((acc, current) => {
          // debugger;
          if (
            current.genre_names &&
            current.genre_names.includes(this.seriesFilterValue)
          ) {
            acc.push(current);
          }
          return acc;
        }, []);
      }
      return this.seriesList;
    },
  },
  mounted() {
    // this.getGenres();
    this.onSearch();
  },
};
</script>

<style>
</style>