<script setup>
import { ref, onMounted } from "vue";
import dayjs from "dayjs";
import advancedFormat from "dayjs/plugin/advancedFormat";
dayjs.extend(advancedFormat);
const data = ref({
  api_key: import.meta.env.VITE_WEATHER_KEY,
  base_url: "https://api.openweathermap.org/data/2.5/",
  query: "London",
  weather: {},
  date: "",
});

const fetchWeather = async () => {
  const apiData = await fetch(
    `${data.value.base_url}weather?q=${data.value.query}&appid=ab66e6608ecccfa0c0512f34910e09d6`
  );
  data.value.weather = await apiData.json();
};
onMounted(() => {
  fetchWeather();
});
const currentDate = () => {
  return dayjs().format(`MMMM Do , YYYY`);
};
</script>

<template>
  <section
    class="text-center mx-auto"
    v-if="data.weather && data.weather.main"
    :class="Math.round(data.weather.main.temp - 273.15) > 16 ? 'warm' : 'cold'"
  >
    <div>
      <label class="relative">
        <input
          type="text"
          placeholder="Search..."
          v-model="data.query"
          @keyup.enter="fetchWeather()"
          class="border-1 p-5 w-2/3 rounded-xl my-20 mx-auto"
        />
        <svg
          class="inline-block absolute"
          height="30px"
          fill="gray"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
        >
          <title>magnify</title>
          <path
            d="M9.5,3A6.5,6.5 0 0,1 16,9.5C16,11.11 15.41,12.59 14.44,13.73L14.71,14H15.5L20.5,19L19,20.5L14,15.5V14.71L13.73,14.44C12.59,15.41 11.11,16 9.5,16A6.5,6.5 0 0,1 3,9.5A6.5,6.5 0 0,1 9.5,3M9.5,5C7,5 5,7 5,9.5C5,12 7,14 9.5,14C12,14 14,12 14,9.5C14,7 12,5 9.5,5Z"
          />
        </svg>
      </label>
    </div>
    <div
      :class="
        Math.round(data.weather.main.temp - 273.15) > 16 ? 'text-white' : ''
      "
    >
      <div v-if="data.weather" class="mb-10">
        <h1 class="text-7xl font-bold mb-10">{{ data.weather.name }}</h1>
        <h2 class="text-2xl font-bold">{{ currentDate() }}</h2>
      </div>
      <div v-if="data.weather && data.weather.main">
        <!-- 在使用 OpenWeatherMap API 時，它返回的氣溫通常以開氏度表示 -->
        <h3 class="mb-8 text-6xl font-bold">
          {{ Math.round(data.weather.main.temp - 273.15) }}°C
        </h3>
        <h3 class="text-3xl font-bold">
          {{ data.weather.weather[0].main }}

          <span
            :class="
              Math.round(data.weather.main.temp - 273.15) > 16 ? 'hidden' : ''
            "
          >
            &#x1f976;
          </span>
          <span
            :class="
              Math.round(data.weather.main.temp - 273.15) > 16 ? '' : 'hidden'
            "
          >
            &#x1f975;
          </span>
        </h3>
      </div>
    </div>
  </section>
</template>

<style scoped>
section {
  max-width: 500px;
  min-height: 950px;
}
svg {
  bottom: -6px;
  right: 15px;
}
.cold {
  background: url(../assets/cold.jpeg) no-repeat;
  background-size: cover;
}
.warm {
  background: url(../assets/warm.jpeg);
  background-size: cover;
}
</style>
