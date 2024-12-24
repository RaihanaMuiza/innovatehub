<template>
  <div class="container-fluid">
    <Navbar @menuSelected="changeView" />
    <!-- <Content :currentView="currentView" /> -->
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
// import Content from "@/components/Content.vue";
// import Products from "@/views/Products.vue";

export default {
  name: "App",
  components: {
    Navbar,
    // Content,
    // Products
  },
  data() {
    return {
      currentView: "Products", // Default view
    };
  },
  methods: {
    changeView(view) {
      if (view === "Products") {
        this.currentView = "Products";
      }
      // Add cases for other views here
    },
  },
};
</script>

<style>
.container-fluid {
  display: flex;
  height: 100vh;
}
</style>
