<template>
  <div>
    <div class="weather-container">
      <h3>{{lowTemp}} &#8457; / {{highTemp}} &#8457;</h3>
      <UmbrellaSVG v-if="rainyWeather" />
      <SunSVG v-else />
    </div>
    <!-- <div v-if="rainyWeather">
      <UmbrellaSVG />
    </div>
    <div v-else>
      <SunSVG />
    </div>-->
  </div>
</template>
<script>
import UmbrellaSVG from "@/assets/icn/umbrella.svg";
import SunSVG from "@/assets/icn/sun.svg";
import axios from "axios";

export default {
  components: {
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
    rainyWeather() {
      //   debugger; // eslint-disable-line
      if (this.weatherCode) {
        const rainyDigits = ["2", "3", "5"];
        const firstDigit = this.weatherCode.toString()[0];
        return rainyDigits.includes(firstDigit);
      } else {
        return false;
      }
    }
  },
  mounted() {
    axios
      .get(
        `http://api.openweathermap.org/data/2.5/onecall?lat=38.7223&lon=9.1393&units=imperial&appid=${process.env.VUE_APP_OPENWEATHER_KEY}`
      )
      .then(response => {
        debugger; // eslint-disable-line
        console.log(response);
        this.weatherCode = response.data.daily[0].weather[0].id;
        this.lowTemp = Math.round(response.data.daily[0].temp.min);
        this.highTemp = Math.round(response.data.daily[0].temp.max);
      });
  }
};
</script>