<template>
    <div class="edit-book">
      <input v-model="editedTitle" @keyup.enter="saveEdit"/>
      <input type="number" v-model.number="editedPages" placeholder="Edit pages"/>
      <button @click="saveEdit">Save</button>
      <button @click="cancelEdit">Cancel</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      initialTitle: String,
      initialPages: Number,
      initialProgress: Number
    },
    data() {
      return {
        editedTitle: this.initialTitle,
        editedPages: this.initialPages
      };
    },
    methods: {
      saveEdit() {
        this.$emit('save-edit', {
            title: this.editedTitle,
            pages: this.editedPages
            });
      },
      cancelEdit() {
        this.$emit('cancel-edit');
      }
    }
  };
  </script>
  
  <style scoped>
  .edit-book {
    padding: 40px;
    max-width: 600px;
    display: flex;
    gap: 5px;
    align-items: center;
    text-align: left;
    justify-content: space-between;
  }
  
  input {
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
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
  
  button:first-of-type {
    background-color: #28a745;
    color: white;
  }
  
  button:last-of-type {
    background-color: #dc3545;
    color: white;
  }
  </style>
  