<template>
  <div class="container-fluid">
    <Navbar @menuSelected="changeView" />
    <Content :currentView="currentView"  /> 
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import Content from "@/components/Content.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Content,
  },
  data() {
    return {
      currentView: "Products", // Default view
    };
  },
  methods: {
    changeView(view) {
      console.log("current view",view);
      this.currentView = view; // Update the current view
    },
  },
};
</script>

<style>
/* Flexbox layout for Navbar and Content */
.container-fluid {
  display: flex;
  height: 100vh; /* Full screen height */
  overflow: hidden; /* Prevent scrolling issues */
}

/* .navbar-container {
  width: 20%; 
  height: 100%; 
  overflow-y: auto; 
} */

.content-container {
  flex-grow: 1; /* Content takes the remaining 80% of the width */
  height: 100%; 
}
</style>