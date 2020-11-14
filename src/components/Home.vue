<template>
  <div class="img-backdrop" style>
    <div class="custom-container">
      <div class="top-nav">
        <h3 @click.stop="toggleLinks" style="display: inline-block;">Links</h3>
        <weather style />
      </div>
      <transition name="fade">
        <links-window v-if="linksActive" v-click-outside="toggleLinks" />
      </transition>
      <!-- absolute center -->
      <div class="banner-time">
        <h3>{{this.currentTime}}</h3>
        <h4>{{this.dateToday}}</h4>
      </div>
      <!-- sidebar right -->
      <div class="metrics-nav"></div>
      <!-- bottom right -->
      <div class="todo-nav"></div>
      <!-- bottom left -->
      <div class="settings-nav"></div>
    </div>
  </div>
</template>
<script>
import LinksWindow from "./LinksWindow.vue";
import Weather from "./Weather.vue";
import dayjs from "dayjs";
// import axios from "axios";

// import utcPlugin from 'dayjs/plugin/utc'
// import isBetween from 'dayjs/plugin/isBetween'
// https://day.js.org/docs/en/display/format

// dayjs.extend(utcPlugin)

export default {
  components: {
    LinksWindow,
    // UmbrellaSVG,
    Weather
  },

  data() {
    return {
      linksActive: false,
      displayStuff: false,
      weatherCode: 0
    };
  },
  methods: {
    toggleLinks() {
      console.log("Hello world!");
      this.linksActive = !this.linksActive;
    }
  },
  computed: {
    dateToday() {
      return dayjs().format("MMMM D");
    },
    currentTime() {
      return dayjs().format("h:mm");
    }
  },
  mounted() {
    // debugger; // eslint-disable-line
  }
};
</script>