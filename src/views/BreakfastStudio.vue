<template>
  <div class="text-gray-900 bg-gray-100 leading-normal">
    <div class="p-4 max-w-5xl mx-auto">
      <Header/>
      <SearchBox
        v-bind:foodGroups="foodGroups"
        v-bind:counter="counter"
        @inputData="updateSearchTerm"
        @tagData="updateSearchTag"
      />
      <FoodCardList v-bind:menuItems="filteredMenuItems" v-bind:counter="counter"/>
      <SearchBox
        v-bind:foodGroups="foodGroups"
        v-bind:counter="counter"
        @inputData="updateSearchTerm"
        @tagData="updateSearchTag"
      />
    </div>
  </div>
</template>

<script>
import { MENU } from "../kitchen";
import FoodCardList from "../components/FoodCardList";
import SearchBox from "../components/SearchBox";
import Header from "../components/Header";

export default {
  name: "BreakfastStudio",
  components: {
    FoodCardList,
    SearchBox,
    Header
  },
  data() {
    return {
      menuItems: MENU,
      searchTerm: "",
      searchTag: "",
      counter: 0
    };
  },
  methods: {
    updateSearchTerm(term) {
      this.searchTerm = term;
    },
    updateSearchTag(tag) {
      this.searchTag = tag;
    }
  },
  computed: {
    foodGroups: function() {
      const allFoodGroups = this.menuItems
        .map(menuItem => menuItem.groups)
        .flat()
        .sort();
      return allFoodGroups.filter((menuItem, index) => {
        const arrayTillIndex = allFoodGroups.slice(0, index);
        return !arrayTillIndex.includes(menuItem);
      });
    },
    filteredMenuItems: function() {
      if (this.searchTerm) {
        // searching only term (no tags)
        if (this.searchTag === "all") {
          return this.menuItems.filter(item => {
            return item.name
              .toLowerCase()
              .includes(this.searchTerm.toLowerCase());
          });
          // searching both by term and tag
        } else {
          return this.menuItems.filter(item => {
            return (
              item.name.toLowerCase().includes(this.searchTerm.toLowerCase()) &&
              item.groups.includes(this.searchTag)
            );
          });
        }
        // searching by tag only
      } else if (this.searchTag && this.searchTag !== "all") {
        return this.menuItems.filter(item => {
          return item.groups.includes(this.searchTag);
        });
        // display all if no search terms or tag selected
      } else {
        return this.menuItems;
      }
    }
  }
};
</script>


<style>
:root {
  --concert-blue: #5fdce3;
}

.grid {
  display: grid;
  grid-gap: 1em;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}

.concert-underline {
  display: inline-block;
  position: relative;
  z-index: 0;
}

.concert-underline::after {
  display: inline-block;
  content: "";
  height: 1em;
  width: 100%;
  background: var(--concert-blue);
  left: 0;
  bottom: 0.25em;
  position: absolute;
  transform: skew(45deg) scale(0.9);
  z-index: -1;
  opacity: 0.5;
}

.concert-underline::before {
  display: inline-block;
  content: "";
  height: 1em;
  width: 100%;
  background: yellow;
  top: 0;
  left: -0.25em;
  position: absolute;
  transform: skew(45deg) scale(0.9) translateX(-0.5em);
  z-index: -1;
  opacity: 0.5;
}
</style>
