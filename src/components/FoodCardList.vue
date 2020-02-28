<template>
  <div class="grid mb-8">
    <div v-for="menuItem in menuItems" v-bind:key="menuItem.name" v-on:click="count">
      <FoodCard v-bind:menuItem="menuItem"/>
    </div>
  </div>
</template>

<script>
import { bus } from "../main";
import FoodCard from "./FoodCard";

export default {
  name: "FoodCardList",
  components: {
    FoodCard
  },
  props: {
    menuItems: {
      type: Array
    },
    counter: {
      type: Number
    }
  },
  data() {
    return {
      updatedCounter: this.counter
    };
  },
  methods: {
    count: function(event) {
      const target = event.target;
      const isIncrementBtn =
        target.classList.contains("add-btn") ||
        target.classList.contains("add-svg");
      const isDecrementBtn =
        target.classList.contains("remove-btn") ||
        target.classList.contains("remove-svg");
      // TODO: because the svg/btn changes onClick to the opposite of itself: add <--> remove
      // Update this logic to be semantic
      if (isIncrementBtn) {
        this.updatedCounter = this.updatedCounter - 1;
        bus.$emit("counterUpdated", this.updatedCounter);
      } else if (isDecrementBtn) {
        this.updatedCounter = this.updatedCounter + 1;
        bus.$emit("counterUpdated", this.updatedCounter);
      }
    }
  }
};
</script>

