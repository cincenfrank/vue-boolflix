<template>
  <div>
    <span v-for="el in rank" :key="el + 'solid'"> ★ </span>
    <span v-for="el in emptyStars" :key="el + 'light'"> ☆ </span>
  </div>
</template>

<script>
export default {
  name: "StarsRank",
  props: {
    rank: Number,
  },
  data() {
    return { maxRank: 5 };
  },
  computed: {
    emptyStars() {
      return this.maxRank - this.rank;
    },
  },
};
</script>

<style>
</style>