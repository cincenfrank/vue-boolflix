<template>
  <div class="media-card">
    <img :src="posterUrl" :alt="title + ' poster'" />
    <div class="card-overlay">
      <p>
        <strong>Title: </strong>
        <span>{{ title }}</span>
      </p>
      <p>
        <strong>Original Language: </strong>
        <span> {{ countryFlag }}</span>
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
    </div>
  </div>
</template>

<script>
import StarsRank from "./StarsRank.vue";
export default {
  components: { StarsRank },
  name: "MediaCard",
  props: {
    title: String,
    originalTitle: String,
    countryCode: String,
    rank: Number,
    posterPath: String,
    overview: String,
  },
  data() {
    return {
      flagObject: {
        it: "🇮🇹",
        en: "🇺🇸",
        es: "🇪🇸",
        de: "🇩🇪",
        fr: "🇫🇷",
        sv: "🇸🇪",
        cs: "🇨🇿",
        pl: "🇵🇱",
        pt: "🇵🇹",
        hi: "🇮🇳",
        tl: "🇵🇭",
        ko: "🇰🇷",
        zh: "🇨🇳",
        hr: "🇭🇷",
        default: "🗺",
      },
      posterBaseUrl: "https://image.tmdb.org/t/p/",
      defaultImageSize: "w342",
    };
  },
  methods: {},
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
        return this.flagObject[this.countryCode.toLowerCase()];
      }
      console.log(this.countryCode);
      return this.flagObject.default;
    },
    rankStarNumber() {
      return Math.round(this.rank / 2);
    },
  },
};
</script>

<style>
</style>