<template>
  <div class="bookshelf-container">
    <div @click="closeBookshelf" class="delete-icon pull-right">x</div>
      <div class="d-flex" style="height: 100%;">
        <div class="books-console">
          <book-search @search-active="toggleSearchStatus" @fetch-books="fetchReadNextBooks" :selected-bestseller="selectedBestseller" />
          <div v-show="!bookSearchActive" class="books-menu">
            <h3 @click="toggleRecentlyRead" class="books-menu__menu-item">Recently Read <span v-show="recentlyReadActive">&#10043;</span></h3>
            <h3 @click="toggleReadingStats" class="books-menu__menu-item">Reading Stats <span v-show="readingStatsActive">&#10043;</span></h3>
            <h3 @click="toggleDatabaseView" class="books-menu__menu-item">Database View <span v-show="databaseViewActive">&#10043;</span></h3>
          </div>
        </div>
        <!-- <books-menu  @close-books-menu="toggleBooksMenu" @fetch-books="fetchBooks" /> -->
        <div v-if="!readingStatsActive && !databaseViewActive" class="d-flex flex-column content-area">
          <!-- <h3>Top 10 Favorites</h3> -->
          <h3>Bestsellers</h3>
          <div v-if="!coverImagesLoaded" class="bookshelf">
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>
            <div class="bookshelf__book-container">
              <div class="bookshelf__book-cover--animated-dummy"></div>
              <div class="bookshelf__book-caption--animated-dummy"></div>
            </div>

          </div>
          <div v-show="coverImagesLoaded" class="bookshelf">
            <!-- uncomment to switch back to top 10 from my personal -->
            <!-- <div class="bookshelf__book-container" v-for="book in readBooks" :key="book.id"> -->
            <book :title="book.title" :author="book.author" :photo="book.photo" @select-bestseller="selectBestseller" @loaded="incrementImageLoaded" v-for="book in bestsellers" :key="book.id" />

            <!-- <div class="bookshelf__book-container" v-for="book in bestsellers" :key="book.id">
              <img v-if="book.photo" class="bookshelf__book-cover" :src="book.photo" @load="incrementImageLoaded" />
              <img v-else class="bookshelf__book-cover" :src="require('../assets/antique_cover_' + 1 + '.jpg')" @load="incrementImageLoaded" />
              <div class="bookshelf__book-caption">
                <p>{{book.title}}</p>
              </div> 
            </div> -->
            <div @click="toggleBestsellersPage" style="color: white;">></div>
          </div>
          <h3>Reading Next</h3>
          <div class="bookshelf">
            <div class="bookshelf__book-container" v-for="book in readNextBooks" :key="book.id">
              <img v-if="book.photo" class="bookshelf__book-cover" :src="book.photo" @load="incrementImageLoaded" />
              <img v-else class="bookshelf__book-cover" :src="require('../assets/antique_cover_' + 1 + '.jpg')" @load="incrementImageLoaded" />
              <div class="bookshelf__book-caption">
                <p>{{book.title}}</p>
              </div> 
              <div @click="deleteReadNextBook(book.id)" class="delete-icon" style="position: absolute; top: 5px; right: -15px;">x</div>
            </div>
          </div>
        </div>
        <reading-stats v-if="readingStatsActive"/>
        <database-view v-if="databaseViewActive"/>
    </div>
  </div>
</template>

<script>
import api from "@/main.js";
import Book from "@/components/Book.vue";
// import BooksMenu from "./BooksMenu.vue"
import BookSearch from "@/components/BookSearch.vue";
import ReadingStats from "@/components/ReadingStats.vue";
import DatabaseView from "@/components/DatabaseView.vue";

export default {
  components: {
    Book,
    BookSearch,
    ReadingStats,
    DatabaseView
  },
  data() {
    return {
      myBooks: [],
      readBooks: [],
      readBooksLoaded: false,
      bookImagesLoaded: 0,
      readNextBooks: [],
      bestsellers: [],
      selectedBestseller: {},
      page: 1,
      bookSearchActive: false,
      recentlyReadActive: true,
      readingStatsActive: false,
      databaseViewActive: false
    }
  },
  methods: {
    fetchBooks(bookType) {
      // debugger; // eslint-disable-line
      if (bookType === 'read_next') {
        api.get('read_next_books')
        .then((response) => {
        console.log(response);
        this.readNextBooks = response.data
        })
      } else {
        api.get('read_books')
        .then((response) => {
        console.log(response);
        this.readBooks = response.data
        })
      }
    },
    fetchReadBooks() {
      api.get('read_books')
      .then((response) => {
        // debugger; //eslint-disable-line
        this.readBooks = response.data.books;
        this.readBooksLoaded = true;
      })
    },
    fetchReadNextBooks() {
      api.get('read_next_books')
      .then((response) => {
        // debugger; //eslint-disable-line
        this.readNextBooks = response.data.books;
      })
    },
    fetchBestsellers() {
      api.get(`bestsellers?page=${this.page}`)
      .then((response) => {
        // debugger; //eslint-disable-line
        this.bestsellers = response.data;
      })
    },
    deleteReadNextBook(id) {
      // debugger; //eslint-disable-line
      api.delete(`books/${id}`)
      .then((response) => {
        console.log(response);
        // debugger; // eslint-disable-line
        const myIndex = this.readNextBooks.findIndex(i => i.id === id);
        this.readNextBooks.splice(myIndex, 1);
      })
      .catch(error => {
        console.log(error);
      })
    },
    closeBookshelf() {
      this.$emit('close-bookshelf');
    },
    toggleSearchStatus(bool) {
      this.bookSearchActive = bool;
    },
    toggleBestsellersPage() {
      if (this.page === 1) {
        this.page = 2;
      } else {
        this.page = 1;
      }
      this.fetchBestsellers();
    },
    toggleReadingStats() {
      this.readingStatsActive = true;
      this.databaseViewActive = false;
      this.recentlyReadActive = false;
    },
    toggleDatabaseView() {
      this.databaseViewActive = true;
      this.readingStatsActive = false;
      this.recentlyReadActive = false;
    },
    toggleRecentlyRead() {
      this.bookSearchActive = false;
      this.recentlyReadActive = true;
      this.readingStatsActive = false;
      this.databaseViewActive = false;
    },
    selectBestseller(data) {
      this.selectedBestseller = data;
      this.bookSearchActive = true;
    },
    incrementImageLoaded() {
      // debugger; // eslint-disable-line
      this.bookImagesLoaded += 1;
    },
    coverRandomizer() {
      return Math.ceil((Math.random() * 3)).toString();
    }

  },
  computed: {
    coverImagesLoaded() {
      // debugger; // eslint-disable-line
      return this.bookImagesLoaded >= 10;
    }
  },
  mounted() {
    this.fetchBestsellers();
    this.fetchReadBooks();
    this.fetchReadNextBooks();
    // const bookCovers = document.querySelectorAll('.bookshelf__book-cover');
    // debugger //eslint-disable-line

    // for (const img of bookCovers) {
    //       debugger //eslint-disable-line
    //   if (img.complete) {
    //     this.incrementImageLoaded();
    //   } else {
    //     img.addEventListener('load', this.incrementImageLoaded);
    //   }
    // }
  }
}
</script>