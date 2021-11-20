<template>
  <div class="title-row d-flex justify-content-between py-3 align-items-center">
    <h2 class="text-white">{{ titleText }}</h2>
    <div>
      <label :for="filterId" class="text-white pe-2"
        >Filter Visible {{ titleText }}</label
      >
      <select
        :name="filterId"
        :id="filterId"
        v-model="selectedValue"
        @change="onSelect"
      >
        <option
          v-for="filterValue in filterList"
          :key="filterValue + '-movie'"
          :value="filterValue"
        >
          {{ filterValue }}
        </option>
      </select>
      <div class="btn btn-danger" @click="onClear">Clear</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TitleRow",
  props: { titleText: String, filterId: String, filterList: Array },
  data() {
    return {
      selectedValue: "",
    };
  },
  methods: {
    onSelect() {
      this.$emit("selectChanged", this.selectedValue);
    },
    onClear() {
      this.selectedValue = "";
      this.onSelect();
    },
  },
};
</script>

<style></style>
