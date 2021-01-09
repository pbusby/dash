<template>
  <div class="img-backdrop" style>
    <div class="custom-container">
      <div class="top-nav">
        <h3 @click.stop="toggleLinks" style="display: inline-block; cursor: pointer;">Links</h3>
        <weather />
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
      <div class="multimedia-nav">
        <BookSVG @click.stop="toggleBooksMenu" />
        <GameSVG />
        <MovieSVG />
      </div>
      
      <!-- bottom right -->
      <div class="todo-nav"></div>
      <!-- bottom left -->
      <div class="settings-nav"></div>
      
    </div>  
    <transition name="drawer">
      <bookshelf @close-bookshelf="toggleBookshelf" v-if="booksActive" />
    </transition>
  </div>
</template>
<script>
import LinksWindow from "./LinksWindow.vue";
import Bookshelf from "./Bookshelf.vue";
import Weather from "./Weather.vue";
import dayjs from "dayjs";
import GameSVG from "@/assets/icn/game.svg";
import BookSVG from "@/assets/icn/book.svg";
import MovieSVG from "@/assets/icn/movie.svg";
// import axios from "axios";

// import utcPlugin from 'dayjs/plugin/utc'
// import isBetween from 'dayjs/plugin/isBetween'
// https://day.js.org/docs/en/display/format

// dayjs.extend(utcPlugin)

export default {
  components: {
    GameSVG,
    BookSVG,
    MovieSVG,
    Bookshelf,
    LinksWindow,
    Weather
  },

  data() {
    return {
      linksActive: false,
      booksActive: false,
      displayStuff: false,
      weatherCode: 0
    };
  },
  methods: {
    toggleLinks() {
      this.linksActive = !this.linksActive;
    },
    toggleBooksMenu() {
      debugger; // eslint-disable-line
      this.booksActive = !this.booksActive;
    },
    toggleBookshelf() {
      this.booksActive = !this.booksActive;
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