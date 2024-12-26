<template>
  <div class="content-section">
    <component :is="currentView" />
  </div>
</template>

<script>
import ProductsView from "@/views/Products.vue"; 

export default {
  name: "Content-section",
  props: ["currentView"], // Dynamically load content based on this prop
  components: {
    ProductsView, 
  },
};
</script>


