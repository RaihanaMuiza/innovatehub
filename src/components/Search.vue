<template>
  <div class="search-container">
    <div class="search-icon">
      <img src="@/assets/search.png" alt="Search Icon" />
    </div>
    <input
      type="text"
      v-model="searchQuery"
      @keyup="onSearch"
      :placeholder="searchPlaceholder"
      class="search-input"
    />
  </div>
</template>

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
  gap: 10px;
  margin-bottom: 20px;
}

.search-icon img {
  width: 20px;
  height: 20px;
}

.search-input {
  flex-grow: 1;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
