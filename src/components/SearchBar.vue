<template>
  <div class="search-bar">
    <input
      class="search-box"
      type="text"
      v-model="inputText"
      @keydown.enter="startSearch"
      placeholder="Search for a movie or a TV show"
    />
    <div class="search-button" @click="startSearch">🔎</div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  data() {
    return {
      inputText: "",
    };
  },
  methods: {
    startSearch() {
      this.$emit("onSearch", this.inputText);
    },
  },
};
</script>

<style>
</style>