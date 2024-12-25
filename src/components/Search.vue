<template>
    <div class="search-container">
      <input
        type="text"
        v-model="searchQuery"
        @keyup="onSearch"
        placeholder="Search for..."
        class="search-input"
      />
    </div>
  </template>
  
  #### **Script**
  ```javascript
  <script>
  export default {
    name: "SearchComponent",
    props: {
      searchPlaceholder: {
        type: String,
        default: "Search...", // Default placeholder text
      },
    },
    data() {
      return {
        searchQuery: "", // Bind user input
      };
    },
    methods: {
      onSearch() {
        this.$emit("search", this.searchQuery); // Emit search query to parent
      },
    },
  };
  </script>
  <style scoped>
  .search-container {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  .search-input {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  </style>
  