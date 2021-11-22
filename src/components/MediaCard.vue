<template>
  <div class="media-card" @mouseover="fetchData">
    <img class="poster-img" :src="posterUrl" :alt="title + ' poster'" />
    <div class="card-overlay">
      <p>
        <strong>Title: </strong>
        <span>{{ title }}</span>
      </p>
      <p>
        <strong>Original Language: </strong>
        <!-- <span> {{ countryFlag }}</span> -->
        <img class="d-inline w-25" :src="countryFlag" alt="dd" />
      </p>
      <p v-if="originalTitle !== title">
        <strong>Original Title: </strong>
        <span> {{ originalTitle }}</span>
      </p>
      <p>
        <strong>Voto: </strong>
        <StarsRank :rank="rankStarNumber" class="stars-rank"></StarsRank>
      </p>
      <strong>Overview </strong>
      <p class="overview">
        <span> {{ overview ? overview : "overview not available" }}</span>
      </p>

      <strong v-if="creditsAlreadyLoaded && castList.length > 0">Cast</strong>
      <!-- <strong>{{ castList.toString() }}</strong> -->
      <div v-if="creditsAlreadyLoaded" class="row row-cols-2">
        <div
          class="col"
          v-for="actor in castList"
          :key="id + '-' + type + '-' + actor.id + 'actor'"
        >
          <ActorCard
            :actorName="actor.name"
            :imageAlt="actor.name + 'profile'"
            :imagePath="profileImageUrl(actor.profile_path)"
          ></ActorCard>
        </div>
      </div>
      <div class="d-flex flex-wrap">
        <GenreChip
          v-for="(genre, i) in genresList"
          :key="'genre-' + i"
          :genreName="genre"
        >
        </GenreChip>
      </div>
    </div>
  </div>
</template>

<script>
import ActorCard from "./ActorCard.vue";
import StarsRank from "./StarsRank.vue";
import axios from "axios";
import GenreChip from "./GenreChip.vue";
export default {
  components: { StarsRank, ActorCard, GenreChip },
  name: "MediaCard",
  props: {
    type: String,
    id: Number,
    title: String,
    originalTitle: String,
    countryCode: String,
    rank: Number,
    posterPath: String,
    overview: String,
    genresList: Array,
  },
  data() {
    return {
      isFetchingData: false,
      creditsAlreadyLoaded: false,
      apiKey: "4315a46b8f5cb187030ac497d365dea0",
      apiEndpoint: "https://api.themoviedb.org/3",
      // flagObject: {
      //   it: "🇮🇹",
      //   en: "🇺🇸",
      //   es: "🇪🇸",
      //   de: "🇩🇪",
      //   fr: "🇫🇷",
      //   sv: "🇸🇪",
      //   cs: "🇨🇿",
      //   pl: "🇵🇱",
      //   pt: "🇵🇹",
      //   hi: "🇮🇳",
      //   tl: "🇵🇭",
      //   ko: "🇰🇷",
      //   zh: "🇨🇳",
      //   hr: "🇭🇷",
      //   default: "🗺",
      // },
      castList: [],
      flagObject: {
        it: "it",
        en: "us",
        es: "es",
        de: "de",
        fr: "fr",
        sv: "sv",
        cs: "cz",
        pl: "pl",
        pt: "pt",
        hi: "in",
        tl: "tl",
        ko: "kr",
        zh: "ch",
        hr: "hr",
        ja: "jp",
        th: "th",
        ru: "ru",
        am: "am",
        default: "xx",
      },
      posterBaseUrl: "https://image.tmdb.org/t/p/",
      defaultImageSize: "w342",
    };
  },
  methods: {
    fetchData() {
      // debugger;
      if (!this.isFetchingData && !this.creditsAlreadyLoaded) {
        this.getCredits();
      }
    },
    profileImageUrl(profilePath) {
      // debugger;
      if (profilePath) {
        return this.posterBaseUrl + "w185" + profilePath;
      } else {
        return require("@/assets/person_placeholder.jpeg");
      }
    },
    getCredits() {
      // debugger;
      this.isFetchingData = true;
      // this.pendingCalls++;
      const creditsSuffix = "/" + this.type + "/";
      axios
        .get(this.apiEndpoint + creditsSuffix + this.id + "/credits", {
          params: { api_key: this.apiKey },
        })
        .then((resp) => {
          // debugger;
          // console.log(resp.data.cast);

          this.castList = [...resp.data.cast];
          this.isFetchingData = false;
          this.creditsAlreadyLoaded = true;

          // this.pendingCalls--;
        });
    },
  },
  computed: {
    posterUrl() {
      if (this.posterPath) {
        return this.posterBaseUrl + this.defaultImageSize + this.posterPath;
      } else {
        return require("@/assets/no_image.jpeg");
      }
    },

    countryFlag() {
      if (this.flagObject[this.countryCode.toLowerCase()]) {
        return require("@/assets/flags/" +
          this.flagObject[this.countryCode.toLowerCase()] +
          ".svg");
      }
      console.log(this.countryCode);
      return require("@/assets/flags/" + this.flagObject.default + ".svg");
    },
    rankStarNumber() {
      return Math.round(this.rank / 2);
    },
  },
  watch: {
    castList: function () {
      this.$forceUpdate();
    },
  },
};
</script>

<style>
</style>