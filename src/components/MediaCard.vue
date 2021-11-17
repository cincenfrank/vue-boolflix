<template>
  <ul>
    <li>
      {{ title }}
      <ul>
        <li>{{ originalTitle }}</li>
        <li>
          {{ getFlag(countryCode) }}
        </li>
        <li>{{ rank }}</li>
      </ul>
    </li>
  </ul>
</template>

<script>
export default {
  name: "MediaCard",
  props: {
    title: String,
    originalTitle: String,
    countryCode: String,
    rank: Number,
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
    };
  },
  methods: {
    getFlag(inputString) {
      if (Object.keys(this.flagObject).includes(inputString)) {
        return this.flagObject[inputString];
      }
      return this.flagObject.default;
    },
  },
};
</script>

<style>
</style>