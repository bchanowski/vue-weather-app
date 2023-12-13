<template>
  <div v-for="city in savedCities" :key="city.index">
    <CityCard :city="city" @click="goToCityView(city)" />
  </div>
  <p v-if="savedCities.length === 0">No locations added.</p>
</template>

<script setup>
import CityCard from "./CityCard.vue";
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
const savedCities = ref([]);
const getCities = async () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
    const requests = [];
    savedCities.value.forEach((city) => {
      requests.push(
        axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=611842a541be2704d8f4d4f25f701a10&units=metric`
        )
      );
    });

    const weatherData = await Promise.all(requests);

    weatherData.forEach((value, index) => {
      savedCities.value[index].weather = value.data;
    });
  }
};

await getCities();
const router = useRouter();
const goToCityView = (city) => {
  router.push({
    name: "city",
    params: {
      state: city.state,
      city: city.city,
    },
    query: {
      id: city.id,
      lng: city.coords.lng,
      lat: city.coords.lat,
    },
  });
};
</script>
