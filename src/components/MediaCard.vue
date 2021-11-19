<template>
  <div class="media-card">
    <img class="poster-img" :src="posterUrl" :alt="title + ' poster'" />
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

      <strong v-if="castList.length > 0">Cast</strong>
      <!-- <strong>{{ castList.toString() }}</strong> -->
      <div class="row row-cols-2">
        <div
          class="col"
          v-for="actor in castList"
          :key="id + '-' + type + '-' + actor.id"
        >
          <ActorCard
            :actorName="actor.name"
            :imageAlt="actor.name + 'profile'"
            :imagePath="profileImageUrl(actor.profile_path)"
          ></ActorCard>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ActorCard from "./ActorCard.vue";
import StarsRank from "./StarsRank.vue";
export default {
  components: { StarsRank, ActorCard },
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
    castList: Array,
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
  methods: {
    profileImageUrl(profilePath) {
      // debugger;
      if (profilePath) {
        return this.posterBaseUrl + "w185" + profilePath;
      } else {
        return require("@/assets/person_placeholder.jpeg");
      }
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
        return this.flagObject[this.countryCode.toLowerCase()];
      }
      console.log(this.countryCode);
      return this.flagObject.default;
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