<template>
    <div class="book-list">
        <h2>Book List</h2>
        <div class="counters">
            <p v-if="bookCount > 0">Completed Books: {{ readCount }} / {{ bookCount }}</p>
        </div>
      <ul>
        <li v-for="(book, index) in books" :key="index">
            <template v-if="book.editing">
                <EditBook
                    :initial-title="book.title"
                    :initial-pages="book.pages"
                    @save-edit="saveEdit(index, $event)"
                    @cancel-edit="cancelEdit(index)"
                />
            </template>
            <template v-else>
                <span :class="{ read: book.read }">{{ book.title }}</span>
                <div class="book-info">
                    <div v-if="book.pages">
                        <h3>Pages read:</h3>
                        <input class="progress" v-model.number="book.progress" type="number"/> 
                        <p>/ {{ book.pages }}</p>
                        <input
                            class="slider"
                            type="range"
                            min="0"
                            :max="book.pages"
                            v-model.number="book.progress"
                            @input="updateProgress(index)"
                        />
                    </div>
                </div>
                <div class="actions">
                    <button class="readButton" @click="setCurrentlyReading(index)" :disabled="book.read">{{ book.currentlyReading ? 'Reading' : 'Read' }}</button>
                    <button @click="startEdit(index)">Edit</button>
                    <button class="deleteButton" @click="deleteBook(index)">Delete</button>
                </div>
            </template>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import EditBook from './EditBook.vue';

  export default {
    components: {
        EditBook
    },

    props: {
      books: Array,
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
      setCurrentlyReading(index) {
        this.$emit('set-currently-reading', index)
      },
      deleteBook(index) {
        this.$emit('delete-book', index)
      },

      startEdit(index) {
        this.$emit('start-edit', index)
      },

      saveEdit(index, editedData) {
        this.$emit('save-edit', index, editedData)
      },

      cancelEdit(index) {
        this.$emit('cancel-edit', index)
      },

      updateProgress(index) {
      this.$emit('update-progress', index, this.books[index].progress);
      }
    }
  };
  </script>
  
  <style scoped>
.book-list {
  margin: 20px auto;
  max-width: 600px;
  text-align: left;
}

.book-list p {
    display: inline-block;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: white;
  margin: 10px 0;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-style: bold;
}

li span {
  flex: 1;
}

.book-info {
  margin-right: 10px;
  display: flex;
  text-align: center;
}

.actions {
  display: flex;
  gap: 5px;
}

button {
  margin: 2px;  
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

.progress {
    border: none;
    text-align: right;
    font-size: medium;
    width: 70px;
    display: inline-block;
}

.readButton {
  background-color: #28a745;
  color: white;
}

.deleteButton {
  background-color: #dc3545;
  color: white;
}

.read {
  text-decoration: line-through;
  color: #888;
}

.slider {
    width: 100px;
    margin-left: 10px;
}
</style>
  