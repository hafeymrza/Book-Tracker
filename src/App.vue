<template>
   <div id="app">
    <h1>Book Reading Tracker</h1>
    <AddBook @book-added="addBook" />
   
    <BookList v-if="bookCount > 0"
      :books="books"
      @set-currently-reading="setCurrentlyReading" 
      @delete-book="deleteBook"
      @start-edit="startEdit"
      @save-edit="saveEdit"
      @cancel-edit="cancelEdit"
      @update-progress="updateProgress" 
    />

  </div>
</template>

<script>
import AddBook from './components/AddBook.vue'
import BookList from './components/BookList.vue'


export default {
  name: 'App',
  components: {
    AddBook, 
    BookList
  },

  data() {
    return {
      books: this.loadBooks()
    }
  },

  computed: {
    bookCount() {
      return this.books.length
    },
    readCount() {
      return this.books.filter(book => book.read).length
    },
  },

  methods: {
    addBook(book) {
      this.books.push({ ...book, read:false, editing: false })
      this.saveBooks()
    },

    setCurrentlyReading(index) {
      this.books.forEach((book, i)=> {
        book.currentlyReading = i === index
      }) 
      this.saveBooks()
    },

    deleteBook(index) {
      this.books.splice(index, 1)
      this.saveBooks()
    },

    startEdit(index) {
      this.books[index].editing = true
      this.saveBooks()
    },

    saveEdit(index, editedData) {
      this.books[index].title = editedData.title
      this.books[index].pages = editedData.pages
      this.books[index].editing = false
      this.saveBooks()
    },

    cancelEdit(index) {
      this.books[index].editing = false
      this.saveBooks()
    },

    updateProgress(index, newProgress) {
      const book = this.books[index]
      book.progress = newProgress
      if (newProgress >= book.pages) {
        book.read = true;
        book.currentlyReading = false;
      } else {
        book.read = false;
      }
      this.saveBooks()
    },

    saveBooks() {
      localStorage.setItem('books', JSON.stringify(this.books))
    },

    loadBooks() {
      const books = localStorage.getItem('books')
      return books ? JSON.parse(books) : []
    }
  }
}
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
}

#app {
  text-align: center;
  margin-top: 50px;
}

h1 {
  color: #333;
}
</style>
