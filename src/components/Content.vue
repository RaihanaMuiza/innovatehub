<template>
  <div class="content-section">
    <component :is="currentView" />
  </div>
</template>

<script>
import ProductsView from "@/views/Products.vue"; // Import the Products component

export default {
  name: "Content-section",
  props: ["currentView"], // Dynamically load content based on this prop
  components: {
    ProductsView, // Register the Products component for dynamic loading
  },
};
</script>

<!-- <style>
.content-section {
  flex-grow: 1;
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin: 10px;
  overflow-y: none;
  min-height: 100vh;
}
</style> -->
