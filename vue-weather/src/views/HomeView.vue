<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchValue"
        @input="getSearchResults"
        placeholder="Search City..."
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-sm placeholder-gray-300"
      />
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResults"
      >
        <p v-if="searchError">Sorry, something went wrong! Try again later.</p>
        <p v-if="!searchError && mapboxSearchResults.length === 0">
          No results match your input!
        </p>
        <template v-else>
          <li
            v-for="searchResult in mapboxSearchResults"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
          >
            {{ searchResult.place_name }}
          </li></template
        >
      </ul>
    </div>
  </main>
</template>
<script setup>
import { ref } from "vue";
import axios from "axios";
const searchValue = ref("");
const searchTimeout = ref(null);
const mapboxAPIKey =
  "pk.eyJ1Ijoiam9obmtvbWFybmlja2kiLCJhIjoiY2t5NjFzODZvMHJkaDJ1bWx6OGVieGxreSJ9.IpojdT3U3NENknF6_WhR2Q";
const mapboxSearchResults = ref(null);
const searchError = ref(null);
const getSearchResults = () => {
  clearTimeout(searchTimeout.value);
  searchTimeout.value = setTimeout(async () => {
    if (searchValue.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchValue.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResults.value = result.data.features;
      } catch {
        searchError.value = true;
      }
      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>
