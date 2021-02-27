<template>
    <div class="dropdown-wrapper">
        <div v-show="!bookSavedMenu" class="dropdown-wrapper__search">
          <!-- book search bar -->
          <div v-if="noBookSelected">
              <h3>Add Book</h3>
              <div class="d-flex align-items-center" style="border-bottom: 1px solid white;">
                  <input style="display: block; border-bottom: none;" ref="dropdowninput" v-model.trim="searchKeyword" @keydown="debouncedCallGoogleBooks" class="dropdown-input" type="text" placeholder="Author name" />
                  <div v-if="apiFetching" class="loading-spinner"></div>
              </div>
          </div>
          <!-- book submit form -->
          <div v-else>
              <!-- <div @click="resetBook" class="dropdown-selected">
              <p class="book-title">{{selectedBook.title}}</p>
              <p class="author">{{selectedBook.author_name[0]}}</p>  
              </div> -->
              <div @click="resetBookSearch" class="delete-icon pull-right">x</div>
              <label for="author-name">Author Name:</label>
              <input v-model="selectedBook.author_name" id="author-name" class="dropdown-input" type="text" />
              <label for="book-title">Book Title:</label>
              <input v-model="selectedBook.title" id="book-title" class="dropdown-input" type="text" />
              <label for="book-genre">Genre:</label>
              <select v-model="selectedBook.genre" id="book-genre" class="dropdown-input">
              <option v-for="genre in genres" :value="genre.keyword" :key="genre.keyword">{{genre.keyword}}</option>
              </select>
              <label for="book-status">Status:</label>
              <select v-model="bookStatus" id="book-status" class="dropdown-input">
              <option v-for="(status, index) in bookStatusOptions" :value="index" :key="index">{{status}}</option>
              </select>
              <!-- <div class="d-flex align-items-center">
              <label for="book-nonfiction">Nonfiction</label>
              <input v-model="bookNonFiction" class="dropdown-input" type="radio" id="book-nonfiction" name="category" />
              <label for="book-fiction">Fiction</label>
              <input v-model="bookFiction" class="dropdown-input" type="radio" id="book-fiction" name="category" />
              </div> -->
              <div :class="this.bookStatus === 1 ? 'disabled-theme' : ''" class="d-flex justify-content-between transition-base">
              <div>
                  <label style="">Score:</label>
                  <div>
                    <span @click="decreaseScore" style="border-bottom: 1px solid white; padding: 8px 6px; display: inline-block;">-</span>
                    <span style="border-bottom: 1px solid white; padding: 8px 0px; display: inline-block;">{{readStatus === 'Already Read' ? bookScore : 'NA'}}</span>
                    <span @click="increaseScore" style="border-bottom: 1px solid white; padding: 8px 5px; display: inline-block;">+</span>
                  </div>
              </div>
              <a @click="submitBook" href="#" class="btn-skeleton mt-3">Save Book</a>
              </div>
          </div>
        </div>
        <!-- book search results -->
        <div v-show="!bookSavedMenu && searchKeyword && apiLoaded" class="dropdown-list">
            <div class="dropdown-list__content">
                <div v-for="result in resultsList" :key="result.name" class="dropdown-item" @click="selectBook(result)">
                  <p class="book-title">{{result.title}}</p>
                  <p class="author">{{result.author_name[0]}}</p>  
                </div>
                <div v-if="apiLoaded && resultsList.length === 0">No books matched that search</div>
            </div>
        </div>
        <!-- book saved / add another menu -->
        <div v-show="bookSavedMenu" class="dropdown-wrapper__saved-menu">
            <div class="d-flex">
                <div class="save-alert">Book saved!</div>
            </div>
            <div class="d-flex justify-content-around">
                <a @click="bookSavedMenu = false" href="#" class="btn mt-3">Add Another</a>
                <!-- add emit event here to close entire books section -->
                <a @click="resetBookSearch" href="#" class="btn mt-3">Finish</a>
            </div>
        </div>
    </div>
</template>
<script>
import _ from "lodash";
import api from "@/main.js"
export default {
    props: {
      selectedBestseller: Object
    },
    data() {
    return {
      addMenuClosed: false,
      bookForm: false,
      bookSearch: false,
      bookSavedMenu: false,
      searchKeyword: "",
      apiLoaded: false,
      apiFetching: false,
      resultsList: [],
      selectedBook: {},
      bookSaved: false,
      bookFiction: false,
      bookNonFiction: false,
      bookStatus: 0,
      bookStatusOptions: ['Already Read', 'Going to Read'],
      myBooks: [],
      readBooks: [],
      toReadBooks: [],
      bookScore: 10,
      genres: []
    };
  },
  methods: {
    callGoogleBooks() {
      console.log("I'm calling yo!");
      if (this.searchKeyword) {
        this.apiFetching = true;
        this.$emit('search-active', true);
        api.get(
        `https://openlibrary.org/search.json?author=${this.searchKeyword}`
        ).then((response) => {
        debugger; // eslint-disable-line
        this.resultsList = response.data.docs.slice(0,20);
        this.apiFetching = false;
        this.apiLoaded = true;
        console.log(response);
      }).catch(error => {
        debugger; // eslint-disable-line
        console.log(error);
      } );
      } 
    },
    selectBook(book) {
      debugger; // eslint-disable-line
      this.selectedBook = book;
      this.searchKeyword = '';
    },
    resetBook() {
      this.selectedBook = {};
      this.$nextTick( () => this.$refs.dropdowninput.focus() )
    },
    fetchGenres() {
      api.get('genres')
      .then((response) => {
              // debugger; // eslint-disable-line
        console.log(response)
        this.genres = response.data
      })
    },
    fetchBooks() {
      api.get('index')
      .then((response) => {
        console.log(response);
        this.myBooks = response.data;
      })
    },
    submitBook() {
      api.post("create_book_record", {
        title: this.selectedBook.title,
        full_name: this.selectedBook.author_name[0],
        cover_url: `https://covers.openlibrary.org/b/id/${this.selectedBook.cover_i}-L.jpg`,
        keyword: this.selectedBook.genre,
        status: this.bookStatus,
        score: this.bookStatus === 0 ? this.bookScore : null
      })
      .then((response) => {
        debugger; // eslint-disable-line
        console.log(response);
        if (response.status === 200) {
          this.bookSavedMenu = true;
          this.resetBookSearch();
          // this.$emit('fetch-books', response.data.status);
          this.$emit('fetch-books');
        }
      })
      .catch((error) => {
        console.log(error);
      });
    },
    resetBookSearch() {
      this.selectedBook = {};
      this.resultsList = [];
      this.bookScore = 10;
      this.apiLoaded = false;
      this.$emit('search-active', false);
    },
    increaseScore() {
      if (this.bookScore < 10) {
        this.bookScore += 1; 
      }
    },
    decreaseScore() {
      if (this.bookScore > 1) {
        this.bookScore -= 1;
      }
    }
  },
  watch: {
    selectedBestseller: function(value) {
      this.selectedBook = value;
      this.searchKeyword = '';
    }
  },
  computed: {
    noBookSelected() {
      return Object.keys(this.selectedBook).length === 0
    },
    readStatus() {
      debugger; // eslint-disable-line
      return this.bookStatus
    }
   
  },
  created() {
    // setting the debounce function here is best practice
    // allows the component to be duplicated with no problems
    // https://v3.vuejs.org/guide/data-methods.html#debouncing-and-throttling
    this.debouncedCallGoogleBooks = _.debounce(this.callGoogleBooks, 1000)
  },
  mounted() {
    this.fetchGenres();
    // this.fetchBooks();
  }
}
</script>