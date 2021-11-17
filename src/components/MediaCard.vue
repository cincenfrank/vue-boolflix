<template>
  <div class="media-card">
    <img :src="posterUrl" :alt="title + ' poster'" />
    <div class="card-overlay">
      <p>
        <strong>Title </strong>
        <span>{{ title }}</span>
      </p>
      <p>
        <strong>Original Country </strong>
        <span> {{ countryFlag }}</span>
      </p>
      <p>
        <strong>Original Title </strong>
        <span> {{ originalTitle }}</span>
      </p>
      <p>
        <strong>Voto </strong>
        <StarsRank :rank="rankStarNumber"></StarsRank>
      </p>
      <p>
        <strong>Overview </strong>
        <span> {{ overview }}</span>
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
        default: "🗺",
      },
      posterBaseUrl: "https://image.tmdb.org/t/p/",
      defaultImageSize: "w342",
    };
  },
  methods: {},
  computed: {
    posterUrl() {
      return this.posterBaseUrl + this.defaultImageSize + this.posterPath;
    },

    countryFlag() {
      if (Object.keys(this.flagObject).includes(this.countryCode)) {
        return this.flagObject[this.countryCode];
      }
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