<template>
  <div class="actor-card">
    <img :src="imagePath" :alt="imageAlt" />
    <p>{{ actorName }}</p>
  </div>
</template>

<script>
export default {
  name: "ActorCard",
  props: {
    imagePath: String,
    imageAlt: String,
    actorName: String,
  },
};
</script>

<style>
</style>