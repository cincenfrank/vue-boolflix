<template>
  <div>
    <TitleRow
      :filterId="isMovie ? 'movieFilter' : 'serieFilter'"
      :filterList="filterList"
      :titleText="shellTitle"
      @selectChanged="filterValue = $event"
    ></TitleRow>
    <p v-if="displayList.length === 0" class="text-white">
      No results found for your search
    </p>
    <div
      v-else-if="displayList.length > 0"
      class="row row-cols-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5"
    >
      <div
        class="col p-0"
        v-for="mediaItem in displayList"
        :key="'movie-' + mediaItem.id"
      >
        <MediaCard
          :id="mediaItem.id"
          :type="isMovie ? 'movie' : 'tv'"
          :title="isMovie ? mediaItem.title : mediaItem.name"
          :countryCode="mediaItem.original_language"
          :originalTitle="
            isMovie ? mediaItem.original_title : mediaItem.original_name
          "
          :rank="mediaItem.vote_average"
          :posterPath="mediaItem.poster_path"
          :overview="mediaItem.overview"
          :genresList="mediaItem.genre_names"
        ></MediaCard>
      </div>
    </div>
  </div>
</template>

<script>
import MediaCard from "./MediaCard.vue";
import TitleRow from "./TitleRow.vue";
export default {
  components: { TitleRow, MediaCard },
  data() {
    return { filterValue: "" };
  },
  props: {
    filterList: Array,
    shellTitle: String,
    isMovie: Boolean,
    mediaItemsList: Array,
  },
  computed: {
    displayList() {
      if (this.filterValue) {
        return this.mediaItemsList.reduce((acc, current) => {
          // debugger;
          if (
            current.genre_names &&
            current.genre_names.includes(this.filterValue)
          ) {
            acc.push(current);
          }
          return acc;
        }, []);
      }
      return this.mediaItemsList;
    },
  },
};
</script>

<style>
</style>