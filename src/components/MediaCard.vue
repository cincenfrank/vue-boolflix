<template>
  <ul>
    <li>
      {{ title }}
      <ul>
        <li>{{ originalTitle }}</li>
        <li>
          {{ countryFlag }}
        </li>
        <!-- <li>{{ rankStarNumber }}</li> -->
        <li>
          <StarsRank :rank="rankStarNumber"></StarsRank>
        </li>
        <li><img :src="backdropUrl" :alt="title + ' poster'" /></li>
      </ul>
    </li>
  </ul>
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
    backdropPath: String,
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
      backdropBaseUrl: "https://image.tmdb.org/t/p/",
      defaultImageSize: "w342",
    };
  },
  methods: {},
  computed: {
    backdropUrl() {
      return this.backdropBaseUrl + this.defaultImageSize + this.backdropPath;
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