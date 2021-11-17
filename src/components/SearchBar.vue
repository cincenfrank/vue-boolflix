<template>
  <div>
    <input type="text" v-model="inputText" />
    <button @click="startSearch">Search</button>
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