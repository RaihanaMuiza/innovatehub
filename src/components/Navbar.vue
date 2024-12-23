<template>
  <div class="sidebar">
    <div class="header">
      <img src="@/assets/logo.png" alt="Logo" class="logo" />
      <span class="company-name">InnovateHub</span>
      <img src="@/assets/user.jpg" alt="User" class="user-icon" />
    </div>
    <div class="menu">
      <div
        v-for="item in menuItems"
        :key="item.name"
        class="menu-item"
        @click="selectMenu(item)"
      >
        <img :src="item.icon" alt="Menu Icon" class="menu-icon" />
        <span class="menu-text">{{ item.name }}</span>
        <span v-if="item.badge" class="badge">{{ item.badge }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Nav-bar",
  data() {
    return {
      menuItems: [
        { name: "Products", icon: "@/assets/products-icon.png", badge: null },
        { name: "Sales", icon: "@/assets/sales-icon.png", badge: "NEW" },
        { name: "Design", icon: "@/assets/design-icon.png", badge: null },
      ],
    };
  },
  methods: {
    selectMenu(item) {
      this.$emit("menuSelected", item.name);
    },
  },
};
</script>

<style>
.sidebar {
  width: 20%;
  height: 100vh;
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
}
.menu-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  cursor: pointer;
}
.menu-item:hover {
  background-color: #f8f9fa;
  border-radius: 5px;
}
.badge {
  background-color: #007bff;
  color: white;
  padding: 5px;
  border-radius: 5px;
  font-size: 12px;
}
</style>
