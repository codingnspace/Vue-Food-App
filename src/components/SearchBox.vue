<template>
  <nav
    class="flex flex-col md:flex-row bg-gray-300 rounded shadow overflow-hidden p-2 mb-8 text-sm"
  >
    <label class="sr-only" for="search">Search</label>
    <input
      class="mb-2 md:mb-0 p-2 rounded flex-grow"
      type="search"
      placeholder="Search for foods"
      id="search"
      v-model="searchTerm"
      @input="submit"
    >
    <select class="mb-2 md:ml-2 md:mb-0" v-model="searchTag" @change="submitTag">
      <option value="all">Food Groups</option>
      <option v-for="option in foodGroups" v-bind:key="option" :value="option">{{option}}</option>
    </select>
    <button
      class="md:ml-2 p-2 rounded"
      style="background-color: var(--concert-blue)"
    >Place Order ({{updatedCounter}})</button>
  </nav>
</template>

<script>
import { bus } from "../main";
export default {
  name: "SearchBox",
  props: {
    foodGroups: {
      type: Array
    },
    counter: {
      type: Number
    }
  },
  data() {
    return {
      searchTerm: "",
      searchTag: "all",
      updatedCounter: this.counter
    };
  },
  methods: {
    submit: function() {
      this.$emit("inputData", this.searchTerm);
    },
    submitTag: function() {
      this.$emit("tagData", this.searchTag);
    }
  },
  created() {
    bus.$on("counterUpdated", newCount => {
      this.updatedCounter = newCount;
    });
  }
};
</script>

<style>
select {
  text-transform: capitalize;
}
</style>