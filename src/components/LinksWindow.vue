<template>
  <div>
    <div class="dropdown-triangle" style="position: absolute; top: 50px; left: 30px"></div>
    <div v-if="favoriteLinkz" style="position: absolute; top: 60px">
      <div class="links-window" ref="linksWindow">
        <div
          class="wrapper"
          :class="{
            'offscreen-left': addLinkActive,
            'onscreen-left': !addLinkActive,
          }"
        >
          <transition name="fade">
            <div class="links-list" ref="linksList">
              <!-- <ul style="color: black"> -->
              <!-- <li v-for="favoriteLink in favoriteLinks" :key="favoriteLink.id"> -->
              <!-- {{ favoriteLink.title }} -->
              <!-- <a @click="deleteFavoriteLink(favoriteLink.id)" href="#">x</a> -->
              <!-- <div class="delete-btn">x</div> -->
              <!-- </li> -->
              <a v-for="favoriteLink in favoriteLinks" :key="favoriteLink.id" :href="favoriteLink.url" target="_blank">
              <div
                class="link-row-wrapper"
              >
                <div class="link-and-icon">
                  <GoogleSVG style="margin-right: 10px;" />
                  <span>{{ favoriteLink.title }}</span>
                </div>
                <div @click="deleteFavoriteLink(favoriteLink.id)" class="icon-wrapper">x</div>
              </div>
              </a>
              <ul>
              <li>cool link 1</li>
              <li>cool link 2</li>
              <li>cool link 3</li>
              <li>cool link 4</li>
              <li>cool link 5</li>
              <li>cool link 6</li>
              <li>cool link 1</li>
              <li>cool link 2</li>
              <li>cool link 3</li>
              <li>cool link 4</li>
              <li>cool link 5</li>
              <li>cool link 6</li>
              <li>cool link 1</li>
              <li>cool link 2</li>
              <li>cool link 3</li>
              <li>cool link 4</li>
              <li>cool link 5</li>
              <li>cool link 6</li>
              <li>cool link 1</li>
              <li>cool link 2</li>
              <li>cool link 3</li>
              <li>cool link 4</li>
              <li>cool link 5</li>
              <li>cool link 6</li>
              </ul>
            </div>
          </transition>
        </div>

        <div
          v-if="favoriteLinkz"
          class="add-link-menu"
          :class="{
            'onscreen-left': addLinkActive,
            'offscreen-left': !addLinkActive,
          }"
        >
          <div @click.stop="deactivateAddLinkMenu" class="back-btn" style="color: white">&larr;</div>
          <form action>
            <label for="Title">TITLE</label>
            <input v-model="formInputTitle" type="text" name="Title" />
            <label for="Title">LINK</label>
            <input v-model="formInputUrl" type="text" label="Link" />
            <a @click="createFavoriteLink" href="#" class="btn mt-3">Create</a>
          </form>
        </div>
      </div>
      <div
        v-if="!addLinkActive"
        class="links-window__add-link-button"
        :class="{
          'onscreen-left': addLinkActive,
          'offscreen-left': !addLinkActive,
        }"
      >
        <h3 style @click.stop="activateAddLinkMenu">+ Add Link</h3>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import GoogleSVG from "@/assets/icn/search.svg";

export default {
  components: {
    GoogleSVG
  },
  data() {
    return {
      linksListActive: true,
      addLinkActive: false,
      favoriteLinks: [],
      formInputTitle: "",
      formInputUrl: "",
      linksListHeight: 0
    };
  },
  methods: {
    activateAddLinkMenu() {
      this.addLinkActive = true;
      this.calculateMenuHeight();
    },
    deactivateAddLinkMenu() {
      this.addLinkActive = false;
      this.calculateMenuHeight();
    },
    calculateMenuHeight() {
      const parent = document.querySelector(".links-window");
      const child1 = document.querySelector(".links-list");
      const child2 = document.querySelector(".add-link-menu");
      //   debugger; // eslint-disable-line
      if (child1.offsetHeight > child2.offsetHeight && !this.addLinkActive) {
        parent.style.height = `${child1.offsetHeight}px`;
      } else {
        parent.style.height = `${child2.offsetHeight}px`;
      }
    },
    createFavoriteLink() {
      axios
        .post("http://localhost:3000/api/v1/favorite_links", {
          title: this.formInputTitle,
          url: this.formInputUrl
        })
        .then(response => {
          console.log(response);
          this.fetchFavoriteLinks();
          this.deactivateAddLinkMenu();
        })
        .catch(error => {
          console.log(error);
        });
    },
    deleteFavoriteLink(id) {
      axios
        .delete(`http://localhost:3000/api/v1/favorite_links/${id}`)
        .then(response => {
          console.log(response);
          this.fetchFavoriteLinks();
          //   this.deactivateAddLinkMenu();
        })
        .catch(error => {
          console.log(error);
        });
    },
    fetchFavoriteLinks() {
      axios
        .get("http://localhost:3000/api/v1/favorite_links")
        .then(response => {
          console.log(response.data);
          this.favoriteLinks = response.data;
          this.$nextTick(() => {
            this.calculateMenuHeight();
          });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  computed: {
    favoriteLinkz() {
      return this.favoriteLinks;
    }
  },

  mounted() {
    axios
      .get("http://localhost:3000/api/v1/favorite_links")
      .then(response => {
        console.log(response.data);
        // debugger; // eslint-disable-line
        this.favoriteLinks = response.data;
        this.$nextTick(() => {
          this.calculateMenuHeight();
        });
      })
      .catch(e => {
        console.log(e);
      });
  }
};
</script>
