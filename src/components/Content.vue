<template>
  <div class="content-section">
    <component :is="currentView" />
  </div>
</template>

<script>
export default {
  name: "Content-section",
  props: ["currentView"],
};
</script>

<style>
.content-section {
  flex-grow: 1;
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin: 10px;
  overflow-y: auto;
  min-height: 100vh;
}
</style>
