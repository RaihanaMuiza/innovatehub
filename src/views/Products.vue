<template>
  <div :class="['products-section', { 'scroll-visible': isSmallScreen }]">
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
          <th><input type="checkbox" @change="selectAll($event)" /></th>
          <th class="brand-column">
            Brand
            <button class="add-icon">+</button>
          </th>
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
            <input
              type="checkbox"
              :checked="product.isSelected"
              @change="toggleCheckbox(product)"
            />
          </td>
          <td class="brand-column">{{ product.brand }}</td>
          <td>
            <span class="description-text" :title="product.description">
              {{ truncateText(product.description, 30) }}</span
            >
          </td>
          <td>
            <div class="members">
              <div
                v-for="(member, index) in getVisibleMembers(product.members)"
                :key="index"
                class="member-container"
              >
                <img
                  :src="require(`@/assets/${member.avatar}`)"
                  :alt="member.name"
                  class="member-avatar"
                />
                <span class="tooltip">{{ member.name }}</span>
              </div>

              <div
                v-if="getRemainingCount(product.members) > 0"
                class="remaining-members-container"
              >
                <span
                  class="remaining-members"
                  @mouseenter="showRemaining = true"
                  @mouseleave="showRemaining = false"
                >
                  +{{ getRemainingCount(product.members) }}
                </span>
                <ul v-if="showRemaining" class="remaining-list">
                  <li
                    v-for="(member, index) in getRemainingMembers(
                      product.members
                    )"
                    :key="index"
                  >
                    {{ member.name }}
                  </li>
                </ul>
              </div>
            </div>
          </td>

          <td>
            <span
              v-for="category in product.categories"
              :key="category"
              class="category-badge"
              :style="{
                backgroundColor: getCategoryColor(category),
                color: 'white',
              }"
            >
              {{ category }}
            </span>
          </td>
          <td>
            <span v-for="tag in product.tags" :key="tag" class="tag-badge">
              {{ tag }}
            </span>
          </td>
          <td>
            <span
              class="meeting-badge"
              :class="getMeetingBadgeClass(product.nextMeeting)"
            >
              {{ product.nextMeeting }}
            </span>
          </td>
          <td></td>
        </tr>
      </tbody>
    </table>

    <div class="products-footer">
      <div class="footer-item">
        <span>Selected</span>
        <span class="footer-badge">{{ selectedCount }}</span>
      </div>

      <div class="footer-item archive">Archive</div>
      <div class="footer-item delete">Delete</div>
      <div class="footer-item more">More</div>
    </div>
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
      isSmallScreen: false,
      products: [
        {
          id: 1,
          brand: "Wix",
          description: "Develop a personalized fitness program.",
          members: [
            { name: "John Doe", avatar: "user.jpg" },
            { name: "Jane Smith", avatar: "user4.jpg" },
          ],
          categories: ["Automation"],
          tags: ["#DigitalTransformation"],
          nextMeeting: "in 30 minutes",
          isSelected: false,
        },
        {
          id: 2,
          brand: "Shopify",
          description: "Introduce a cloud-based retail solution.",
          members: [
            { name: "Patty", avatar: "user7.jpg" },
            { name: "Patty", avatar: "user3.jpg" },
          ],
          categories: ["E-commerce", "B2B"],
          tags: ["#OnlineShopping", "#Digital"],
          nextMeeting: "Tomorrow",
          isSelected: false,
        },
        {
          id: 3,
          brand: "MailChimp",
          description:
            "Develop a mobile application to track the mails for your cutomers",
          members: [
            { name: "Alice", avatar: "user2.jpg" },
            { name: "Wane", avatar: "user4.jpg" },
            { name: "July", avatar: "user6.jpg" },
            { name: "Roselwe", avatar: "user8.jpg" },
            { name: "Patty", avatar: "user7.jpg" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "Tomorrow",
          isSelected: false,
        },
        {
          id: 5,
          brand: "PayPal",
          description: "This program could include financial services.",
          members: [{ name: "Mane Fraser", avatar: "user5.jpg" }],
          categories: ["Marketplace"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "In 6 hours",
          isSelected: false,
        },
        {
          id: 6,
          brand: "Disney",
          description: "Introduce a B2B Solution",
          members: [
            { name: "Alice", avatar: "user8.jpg" },
            { name: "Maryn", avatar: "user3.jpg" },
            { name: "Tress", avatar: "user7.jpg" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "No Contact",
          isSelected: false,
        },
        {
          id: 7,
          brand: "Intercom",
          description: "Implement an AI driven solution",
          members: [
            { name: "Alice", avatar: "user4.jpg" },
            { name: "Ewan", avatar: "user3.jpg" },
            { name: "Teen", avatar: "user6.jpg" },
            { name: "Henry", avatar: "user.jpg" },
            { name: "Patty", avatar: "user2.jpg" },
            { name: "Wayne", avatar: "user5.jpg" },
            { name: "Liza", avatar: "user8.jpg" },
          ],
          categories: ["Finance", "Automation"],
          tags: ["#SmartFinance", "#Workflow"],
          nextMeeting: "In 1 hour",
          isSelected: false,
        },
        {
          id: 8,
          brand: "Google",
          description: "Offer a comprehensive solution",
          members: [
            { name: "Henry", avatar: "user4.jpg" },
            { name: "Alice", avatar: "user2.jpg" },
            { name: "Jayne", avatar: "user.jpg" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "In 30 minutes",
          isSelected: false,
        },
        {
          id: 9,
          brand: "Evernote",
          description: "The cloud include smart list",
          members: [
            { name: "Alice", avatar: "user8.jpg" },
            { name: "Raene", avatar: "user5.jpg" },
            { name: "Oss", avatar: "user3.jpg" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "next Month",
          isSelected: false,
        },
        {
          id: 10,
          brand: "Microsoft",
          description: "Launch advisory service",
          members: [
            { name: "Alice", avatar: "user2.jpg" },
            { name: "Wayne", avatar: "user6.jpg" },
            { name: "Irend", avatar: "user5.jpg" },
            { name: "Neen", avatar: "user8.jpg" },
            { name: "Ugeane", avatar: "user.jpg" },
            { name: "Keller", avatar: "user4.jpg" },
          ],
          categories: ["Publishing", "B2B"],
          tags: ["#B2CMarketing", "#Retail"],
          nextMeeting: "No contact",
          isSelected: false,
        },
        {
          id: 11,
          brand: "Invision",
          description: "The tool would analyze the data",
          members: [
            { name: "Alice", avatar: "user5.jpg" },
            { name: "Jerry", avatar: "user2.jpg" },
          ],
          categories: ["Publishing", "B2B"],
          tags: ["#B2CMarketing", "#Retail"],
          nextMeeting: "Next Month",
          isSelected: false,
        },
      ],
      filteredProducts: [],
      categoryColors: {
        SAAS: "#fdd835", // Yellow
        Publishing: "#66bb6a", // Green
        Automation: "#42a5f5", // Blue
        ECommerce: "#ab47bc", // Purple
        B2B: "#ef5350", // Red
        Mobile: "#ec407a", // Pink
        Marketplace: "#26a69a", // Teal
        Finance: "#ab47bc",
      },
      showRemaining: false,
      selectedCount: 0, // Tracks the number of selected checkboxes
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
    truncateText(text, maxLength) {
      if (text.length > maxLength) {
        return text.slice(0, maxLength) + "...";
      }
      return text;
    },
    getMeetingBadgeClass(nextMeeting) {
      const meetingText = nextMeeting.toLowerCase();

      if (
        meetingText.includes("today") ||
        meetingText.includes("minutes") ||
        meetingText.includes("hours") ||
        meetingText.includes("hour")
      ) {
        return "badge-green";
      } else if (meetingText.includes("tomorrow")) {
        return "badge-blue";
      } else if (meetingText.includes("no contact")) {
        return "badge-red";
      } else if (meetingText.includes("next month")) {
        return "badge-grey";
      } else {
        return "badge-grey";
      }
    },
    checkScreenSize() {
      this.isSmallScreen = window.innerWidth <= 768;
    },
    getCategoryColor(category) {
      return this.categoryColors[category] || "#e0e0e0";
    },
    getVisibleMembers(members) {
      return members.slice(0, 4); // Show the first 4 members
    },
    getRemainingCount(members) {
      return members.length > 4 ? members.length - 4 : 0;
    },
    getRemainingMembers(members) {
      return members.slice(4); // Get the remaining members after the first 4
    },
    toggleCheckbox(product) {
      product.isSelected = !product.isSelected;
      this.selectedCount = this.products.filter((p) => p.isSelected).length;
    },
    selectAll(event) {
      const isChecked = event.target.checked;
      this.products.forEach((product) => {
        product.isSelected = isChecked;
      });
      this.selectedCount = isChecked ? this.products.length : 0;
    },
  },
  mounted() {
    this.checkScreenSize();
    window.addEventListener("resize", this.checkScreenSize);

    this.filteredProducts = this.products;
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.checkScreenSize);
  },
};
</script>

<style scoped>
@import "@/styles/products.css";
</style>
