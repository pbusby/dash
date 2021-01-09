<template>
  <div>
    <div class="weather-container">
      <h3>{{lowTemp}} &#8457; / {{highTemp}} &#8457;</h3>
      <UmbrellaSVG v-if="weatherStatus === 'Rainy'" />
      <CloudSVG v-if="weatherStatus === 'Cloudy'" />
      <SunSVG v-if="weatherStatus === 'Clear'" />
    </div>
  </div>
</template>
<script>
import UmbrellaSVG from "@/assets/icn/umbrella.svg";
import CloudSVG from "@/assets/icn/cloud.svg";
import SunSVG from "@/assets/icn/sun.svg";
import axios from "axios";

export default {
  components: {
    CloudSVG,
    SunSVG,
    UmbrellaSVG
  },
  data() {
    return {
      weatherCode: 0,
      highTemp: 0,
      lowTemp: 0
    };
  },
  methods: {},
  computed: {
    weatherStatus() {
      // list of weather codes
      // https://openweathermap.org/weather-conditions#Weather-Condition-Codes-2
      if (this.weatherCode) {
        const rainyStatuses = ['Rain', 'Thunderstorm', 'Drizzle'];
        const cloudyStatuses = ['Cloudy'];
        if (rainyStatuses.includes(this.weatherCode)) {
          return 'Rainy';
        } else if (cloudyStatuses.includes(this.weatherCode)) {
          return 'Cloudy';
        } else if (this.weatherCode === 'Clear') {
          return 'Clear';
        } else {
          return false;
        }
      } else {
        return false;
      }
    }
  },
  mounted() {
    axios
      .get(
        `https://api.openweathermap.org/data/2.5/onecall?lat=38.7223&lon=9.1393&units=imperial&appid=${process.env.VUE_APP_OPENWEATHER_KEY}`
      )
      .then(response => {
        // debugger; // eslint-disable-lint
        console.log(response);
        this.weatherCode = response.data.daily[0].weather[0].main;
        this.lowTemp = Math.round(response.data.daily[0].temp.min);
        this.highTemp = Math.round(response.data.daily[0].temp.max);
      });
  }
};
</script>