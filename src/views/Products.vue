<template>
  <div class="products-section">
    <!-- Page Header -->
    <div class="products-header">
      <h2>Products</h2>
      <div class="header-tools">
        <Search @search="filterProducts" />
        <button class="icon-button">
          <img src="@/assets/message.png" alt="Message Icon" />
        </button>
        <button class="icon-button">
          <img src="@/assets/setting.png" alt="Settings Icon" />
        </button>
      </div>
    </div>
    <hr class="divider" />
    <!-- Toolbar Row -->
    <div class="products-toolbar">
      <div class="toolbar-left">
        <button class="toolbar-button">All brands</button>
        <button class="toolbar-button">Desk</button>
        <button class="toolbar-button">Tags</button>
        <button class="toolbar-button">Sort</button>
        <button class="toolbar-button">Filter</button>
      </div>
      <div class="toolbar-right">
        <button class="toolbar-badge">Meeting</button>
        <button class="toolbar-badge">Import/Export</button>
      </div>
    </div>
    <hr class="divider" />
    <!-- Products Table -->
    <table class="products-table">
      <thead>
        <tr>
          <th></th>
          <th>Brand</th>
          <th>Description</th>
          <th>Members</th>
          <th>Categories</th>
          <th>Tags</th>
          <th>Next Meeting</th>
          <th>+</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in filteredProducts" :key="product.id">
          <td>
            <input type="checkbox" />
          </td>
          <td>{{ product.brand }}</td>
          <td>{{ product.description }}</td>
          <td>
            <div class="members">
              <img
                v-for="member in product.members"
                :key="member.name"
                :src="member.avatar"
                :alt="member.name"
                class="member-avatar"
              />
            </div>
          </td>
          <td>
            <span
              v-for="category in product.categories"
              :key="category"
              class="category-badge"
            >
              {{ category }}
            </span>
          </td>
          <td>
            <span v-for="tag in product.tags" :key="tag" class="tag-badge">
              {{ tag }}
            </span>
          </td>
          <td>{{ product.nextMeeting }}</td>
          <td>
            <!-- <button class="add-button">+</button> -->
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Search from "@/components/Search.vue";

export default {
  name: "ProductsView",
  components: {
    Search,
  },
  data() {
    return {
      searchQuery: "",
      products: [
        {
          id: 1,
          brand: "Wix",
          description: "Develop a personalized fitness program.",
          members: [
            { name: "John Doe", avatar: "@/assets/user.jpg" },
            { name: "Jane Smith", avatar: "path/to/avatar2.png" },
          ],
          categories: ["Automation"],
          tags: ["#DigitalTransformation"],
          nextMeeting: "in 30 minutes",
        },
        {
          id: 2,
          brand: "Shopify",
          description: "Introduce a cloud-based retail solution.",
          members: [{ name: "Alice", avatar: "path/to/avatar3.png" }],
          categories: ["E-commerce", "B2B"],
          tags: ["#OnlineShopping", "#Digital"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 3,
          brand: "MailChimp",
          description: "Develop a mobile application.",
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Wane", avatar: "path/to/avatar3.png" },
            { name: "July", avatar: "path/to/avatar3.png" },
            { name: "Roselwe", avatar: "path/to/avatar3.png" },
            { name: "Patty", avatar: "path/to/avatar3.png" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 4,
          brand: "MailChimp",
          description: "Develop a mobile application.",
          members: [{ name: "Alice", avatar: "path/to/avatar3.png" }],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 5,
          brand: "PayPal",
          description: "This program could include financial services.",
          members: [{ name: "Mane Fraser", avatar: "path/to/avatar3.png" }],
          categories: ["Marketplace"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 6,
          brand: "Disney",
          description: "Introduce a B2B Solution",
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Maryn", avatar: "path/to/avatar3.png" },
            { name: "Tress", avatar: "path/to/avatar3.png" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 7,
          brand: "Intercom",
          description: "Implement an AI driven solution",
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Alice", avatar: "path/to/avatar3.png" },
          ],
          categories: ["Finance", "Automation"],
          tags: ["#SmartFinance", "#Workflow"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 8,
          brand: "Google",
          description: "Offer a comprehensive solution",
          members: [{ name: "Alice", avatar: "path/to/avatar3.png" }],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 9,
          brand: "Evernote",
          description: "The cloud include smart list",
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Raene", avatar: "path/to/avatar3.png" },
            { name: "Oss", avatar: "path/to/avatar3.png" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 10,
          brand: "Microsoft",
          description: "Launch advisory service",
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Wayne", avatar: "path/to/avatar3.png" },
            { name: "Irend", avatar: "path/to/avatar3.png" },
            { name: "Ugeane", avatar: "path/to/avatar3.png" },
          ],
          categories: ["Publishing", "B2B"],
          tags: ["#B2CMarketing", "#Retail"],
          nextMeeting: "Tomorrow",
        },
        {
          id: 11,
          brand: "Invision",
          description: "The tool would analyze the data",
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
            { name: "Alice", avatar: "path/to/avatar3.png" },
          ],
          categories: ["Publishing", "B2B"],
          tags: ["#B2CMarketing", "#Retail"],
          nextMeeting: "Tomorrow",
        },
      ],
      filteredProducts: [],
    };
  },
  methods: {
    filterProducts(query) {
      this.searchQuery = query.toLowerCase();
      this.filteredProducts = this.products.filter(
        (product) =>
          product.brand.toLowerCase().includes(this.searchQuery) ||
          product.description.toLowerCase().includes(this.searchQuery) ||
          product.categories.some((cat) =>
            cat.toLowerCase().includes(this.searchQuery)
          ) ||
          product.tags.some((tag) =>
            tag.toLowerCase().includes(this.searchQuery)
          )
      );
    },
  },
  mounted() {
    this.filteredProducts = this.products; // Initialize with all products
  },
};
</script>

<style scoped>
.products-section {
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin: 10px;
  overflow-y: hidden;
  height: calc(100vh - 40px);
}

.products-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-tools {
  display: flex;
  align-items: center;
  gap: 10px;
}

.icon-button {
  width: 35px;
  height: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f1f1f1;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: -15px;
}

.products-toolbar {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.toolbar-button {
  padding: 5px 10px;
  background-color: #f1f1f1;
  border: 1px solid #ddd;
  border-radius: 5px;
  cursor: pointer;
}

.products-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
}

.products-table th,
.products-table td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.add-button {
  background-color: #f1f1f1;
  border: none;
  padding: 5px;
  border-radius: 5px;
  cursor: pointer;
}

.members {
  display: flex;
}

.member-avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-right: 5px;
}

.category-badge,
.tag-badge {
  display: inline-block;
  background-color: #f0f0f0;
  color: #333;
  padding: 5px 10px;
  border-radius: 10px;
  font-size: 0.9rem;
  margin: 2px;
}

.products-toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

/* Left buttons */
.toolbar-left {
  display: flex;
  gap: 10px;
}

.toolbar-button {
  padding: 5px 10px;
  background-color: #f1f1f1;
  border: 1px solid #ddd;
  border-radius: 15px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: bold;
}

/* Right badges */
.toolbar-right {
  display: flex;
  gap: 10px;
}

.toolbar-badge {
  padding: 5px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: bold;
}

.toolbar-badge:hover {
  background-color: #0056b3;
}
</style>
