<template>
  <div class="products-section">
    <!-- Page Header -->
    <div class="products-header">
      <h2>Products</h2>
      <Search @search="filterProducts" />
    </div>

    <!-- Products Table -->
    <table class="products-table">
      <thead>
        <tr>
          <th>Brand</th>
          <th>Description</th>
          <th>Members</th>
          <th>Categories</th>
          <th>Tags</th>
          <th>Next Meeting</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in filteredProducts" :key="product.id">
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
            <span
              v-for="tag in product.tags"
              :key="tag"
              class="tag-badge"
            >
              {{ tag }}
            </span>
          </td>
          <td>{{ product.nextMeeting }}</td>
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
            { name: "John Doe", avatar: "path/to/avatar1.png" },
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
          members: [
            { name: "Alice", avatar: "path/to/avatar3.png" },
          ],
          categories: ["E-commerce", "B2B"],
          tags: ["#OnlineShopping", "#Digital"],
          nextMeeting: "Tomorrow",
        },
        // Add more product rows here
      ],
      filteredProducts: [],
    };
  },
  methods: {
    filterProducts(query) {
      this.searchQuery = query.toLowerCase();
      this.filteredProducts = this.products.filter((product) =>
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
  overflow-y: auto;
  height: calc(100vh - 40px); /* Adjust height as per layout */
}

.products-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
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
</style>

