<template>
  <div>
    <div
      class="dropdown-triangle"
      style="position: absolute; top: 30px; left: 30px"
    ></div>
    <div style="position: absolute; top: 40px">
      <div class="links-window">
        <div
          class="links-list"
          :class="{
            'offscreen-left': addLinkActive,
            'onscreen-left': !addLinkActive,
          }"
        >
          <ul style="color: black">
            <li v-for="favoriteLink in favoriteLinks" :key="favoriteLink.id">
              {{ favoriteLink.title }}
              <a @click="deleteFavoriteLink(favoriteLink.id)" href="#">x</a>
            </li>
            <!-- <li>cool link 1</li>
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
                        <li>cool link 6</li> -->
          </ul>
        </div>

        <div
          class="add-link-menu"
          :class="{
            'onscreen-left': addLinkActive,
            'offscreen-left': !addLinkActive,
          }"
        >
          <!-- <label>Title</label> -->
          <div
            @click.stop="deactivateAddLinkMenu"
            class="back-btn"
            style="color: black"
          >
            &larr;
          </div>
          <form action>
            <label for="Title">Title</label>
            <input v-model="formInputTitle" type="text" name="Title" />
            <label for="Title">Link</label>
            <input v-model="formInputUrl" type="text" label="Link" />
            <a @click="createFavoriteLink" href="#" class="btn">Create</a>
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
        <h3 style="" @click.stop="activateAddLinkMenu">+ Add Link</h3>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      linksListActive: true,
      addLinkActive: false,
      favoriteLinks: [],
      formInputTitle: "",
      formInputUrl: "",
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
      // debugger // eslint-disable-line
      const parent = document.querySelector(".links-window");
      const child1 = document.querySelector(".links-list");
      const child2 = document.querySelector(".add-link-menu");
      if (child1.offsetHeight > child2.offsetHeight && !this.addLinkActive) {
        // return child1.offsetHeight;
        parent.style.height = `${child1.offsetHeight}px`;
      } else {
        // return child2.offsetHeight;
        parent.style.height = `${child2.offsetHeight}px`;
      }
      // var parentHeight = $('#parent').height(),
      // childHeight = $('#child').height();

      // if (parentHeight <= childHeight) {
      // $('#parent').height(childHeight);
    },
    createFavoriteLink() {
      axios
        .post("http://localhost:3000/api/v1/favorite_links", {
          title: this.formInputTitle,
          url: this.formInputUrl,
        })
        .then((response) => {
          console.log(response);
          this.fetchFavoriteLinks();
          this.deactivateAddLinkMenu();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    deleteFavoriteLink(id) {
      axios
        .delete(`http://localhost:3000/api/v1/favorite_links/${id}`)
        .then((response) => {
          console.log(response);
          this.fetchFavoriteLinks();
          //   this.deactivateAddLinkMenu();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    fetchFavoriteLinks() {
      axios
        .get("http://localhost:3000/api/v1/favorite_links")
        .then((response) => {
          console.log(response.data);
          debugger; // eslint-disable-line
          this.favoriteLinks = response.data;
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  computed: {
    magicalHeight() {
      // debugger // eslint-disable-line
      // const parent = document.querySelector('.links-window');
      const child1 = document.querySelector(".links-list");
      const child2 = document.querySelector(".add-link-menu");
      if (child1 && child2 && child1.offsetHeight > child2.offsetHeight) {
        return child1.offsetHeight;
      } else {
        return child2.offsetHeight;
      }
    },
  },

  mounted() {
    // debugger // eslint-disable-line
    this.calculateMenuHeight();

    return axios
      .get("http://localhost:3000/api/v1/favorite_links")
      .then((response) => {
        console.log(response.data);
        debugger; // eslint-disable-line
        this.favoriteLinks = response.data;
      })
      .catch((e) => {
        console.log(e);
      });
  },
};
</script>
