<template>
  <v-card>
    <v-card-title>
      Books
      <v-spacer></v-spacer>
      <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
          @input="searchAllBy"
      ></v-text-field>
      <v-btn @click="addBook">Add Book</v-btn>
    </v-card-title>
    <v-data-table
        :headers="headers"
        :items="books"
        @click:row="editBook"
        @refresh="refreshList"
    ></v-data-table>
    <BookDialog
        :opened="dialogVisible"
        :book="selectedBook"
        @refresh="refreshList"
    ></BookDialog>
  </v-card>
</template>

<script>
import api from "../api";
import BookDialog from "../components/BookDialog";
export default {
  name: "BookList",
  components: { BookDialog },
  data() {
    return {
      books: [],
      search: "",
      headers: [
        {
          text: "Title",
          align: "start",
          sortable: false,
          value: "title",
        },
        { text: "Author", value: "author" },
        { text: "Genre", value: "genre" },
        { text: "Quantity", value: "quantity" },
        { text: "Price", value: "price" },
      ],
      dialogVisible: false,
      selectedBook: {},
    };
  },
  methods: {
    addBook() {
      this.dialogVisible = true;
    },
    editBook(book) {
      this.selectedBook = book;
      this.dialogVisible = true;
    },
    async refreshList() {
      this.dialogVisible = false;
      this.selectedBook = {};
      this.books = await api.books.allBooks();
    },
    async searchAllBy() {
      if (this.search !== "") {
        this.books = await api.books.filter(this.search);
      } else {
        this.search = "";
        this.books = await api.books.allBooks();
      }
    },
  },
  created() {
    this.refreshList();
  },
};
</script>

<style scoped></style>