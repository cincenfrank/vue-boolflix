<template>
  <header>
    <nav>
      <img src="@/assets/logo_boolflix.png" alt="logo boolflix" />
      <SearchBar @onSearch="onSearch"></SearchBar>
    </nav>
  </header>
</template>

<script>
import SearchBar from "./SearchBar.vue";
export default {
  name: "Header",
  components: { SearchBar },
  methods: {
    onSearch(searchedText) {
      this.$emit("onSearch", searchedText);
    },
  },
};
</script>
    

<style>
</style>