<template>
    <div class="add-book">
      <h2>Add a Book</h2>
      <form @submit.prevent="addBook">
        <input v-model="title" type="text" placeholder="Book Title" required />
        <input v-model.number="pages" type="number" placeholder="Number of pages"/>
        <button type="submit">Add Book</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        title: '',
        pages: null
      };
    },
    methods: {
        addBook() {
            if (this.title && this.pages) {
                this.$emit('book-added', { title: this.title, pages: this.pages, progress: 0});
                this.title = '';
                this.pages = null;
                this.progress = 0;
            } else {
                alert('Please enter both title and number of pages.');
            }
        }
    }
  };
  </script>
  
  <style scoped>
  .add-book {
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  form {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  input {
    padding: 8px;
    margin-right: 10px;
    border: 1px solid #ccc;
    border-radius: 4px
  }

  button {
    padding: 10px 15px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
  opacity: 0.8;
}
  </style>
  