<template>
  <div class="rounded-pill bg-info d-inline-block p-2 m-2 text-dark">
    {{ genreName }}
  </div>
</template>

<script>
export default {
  name: "GenreChip",
  props: { genreName: String },
};
</script>

<style>
</style>