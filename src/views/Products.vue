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
          <th><input type="checkbox" /></th>
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
            <input type="checkbox" />
          </td>
          <td class="brand-column">{{ product.brand }}</td>
          <td>
            <span class="description-text" :title="product.description">
              {{ truncateText(product.description, 30) }}</span
            >
          </td>
          <td>
            <div class="members">
              <img
                v-for="(member, index) in getVisibleMembers(product.members)"
                :key="index"
                :src="require(`@/assets/${member.avatar}`)"
                :alt="member.name"
                class="member-avatar"
              />
              <span
                v-if="getRemainingCount(product.members) > 0"
                class="remaining-members"
              >
                +{{ getRemainingCount(product.members) }}
              </span>
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
        },
        {
          id: 5,
          brand: "PayPal",
          description: "This program could include financial services.",
          members: [{ name: "Mane Fraser", avatar: "user5.jpg" }],
          categories: ["Marketplace"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "In 6 hours",
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
        },
        {
          id: 7,
          brand: "Intercom",
          description: "Implement an AI driven solution",
          members: [
            { name: "Alice", avatar: "user4.jpg" },
            { name: "Alice", avatar: "user3.jpg" },
            { name: "Alice", avatar: "user6.jpg" },
            { name: "Alice", avatar: "user.jpg" },
            { name: "Patty", avatar: "user2.jpg" },
            { name: "Patty", avatar: "user5.jpg" },
            { name: "Patty", avatar: "user8.jpg" },
          ],
          categories: ["Finance", "Automation"],
          tags: ["#SmartFinance", "#Workflow"],
          nextMeeting: "In 1 hour",
        },
        {
          id: 8,
          brand: "Google",
          description: "Offer a comprehensive solution",
          members: [
            { name: "Alice", avatar: "user4.jpg" },
            { name: "Alice", avatar: "user2.jpg" },
            { name: "Alice", avatar: "user.jpg" },
          ],
          categories: ["SAAS", "Mobile"],
          tags: ["#TechInnovation", "#CloudComputing"],
          nextMeeting: "In 30 minutes",
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
        },
        {
          id: 10,
          brand: "Microsoft",
          description: "Launch advisory service",
          members: [
            { name: "Alice", avatar: "user2.jpg" },
            { name: "Wayne", avatar: "user6.jpg" },
            { name: "Irend", avatar: "user5.jpg" },
            { name: "Ugeane", avatar: "user8.jpg" },
            { name: "Ugeane", avatar: "user.jpg" },
            { name: "Ugeane", avatar: "user4.jpg" },
          ],
          categories: ["Publishing", "B2B"],
          tags: ["#B2CMarketing", "#Retail"],
          nextMeeting: "No contact",
        },
        {
          id: 11,
          brand: "Invision",
          description: "The tool would analyze the data",
          members: [
            { name: "Alice", avatar: "user5.jpg" },
            { name: "Alice", avatar: "user2.jpg" },
          ],
          categories: ["Publishing", "B2B"],
          tags: ["#B2CMarketing", "#Retail"],
          nextMeeting: "Next Month",
        },
      ],
      filteredProducts: [],
      categoryColors: {
        SAAS: "#fdd835", // Yellow
        "Cloud Services": "#66bb6a", // Green
        Automation: "#42a5f5", // Blue
        ECommerce: "#ab47bc", // Purple
        B2B: "#ef5350", // Red
        Mobile: "#ec407a", // Pink
      },
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
      // const today = new Date();
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
      return this.categoryColors[category] || "#e0e0e0"; // Default color for unknown categories
    },
    getVisibleMembers(members) {
      return members.slice(0, 4); // Show the first 4 members
    },
    getRemainingCount(members) {
      return members.length > 4 ? members.length - 4 : 0;
    },
  },
  mounted() {
    this.checkScreenSize();
    window.addEventListener("resize", this.checkScreenSize);

    this.filteredProducts = this.products; // Initialize with all products
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.checkScreenSize);
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
  overflow-y: auto; /* Enable vertical scrolling */
  max-height: 100vh; /* Ensure it doesn't exceed the viewport height */
  transition: all 0.3s ease;
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
  margin-top: 15px;
  border-radius: 5px;
  border: 1px solid #f3f2f2;
}

.products-table th,
.products-table td {
  padding: 5px;
  border: 0.5px solid #f3f2f2;
}

.productd-table td {
  text-align: center;
  vertical-align: middle;
  padding: 10px;
}

.brand-column {
  width: 17.5%;
  position: relative;
}

.add-button {
  background-color: #f1f1f1;
  border: none;
  padding: 5px;
  border-radius: 5px;
  cursor: pointer;
}

.add-icon {
  float: right; /* Align the + icon to the right */
  background: none;
  border: none;
  color: #007bff;
  cursor: pointer;
}

.add-icon:hover {
  color: #0056b3;
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
  font-size: 0.8rem;
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
  border-radius: 7.5px;
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
  background-color: #f1f1f1;
  color: black;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: bold;
}

.toolbar-badge:hover {
  background-color: #0056b3;
}

/* Description Column */
.description-column {
  width: 30%; /* Fixed width for description */
  white-space: nowrap; /* Prevent text wrapping */
  overflow: hidden;
  text-overflow: ellipsis; /* Display ellipsis for overflow text */
}

.description-text {
  display: inline-block;
  max-width: 100%; /* Ensure truncation works */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  cursor: pointer; /* Indicate hover for tooltip */
}

/* .description-text:hover {
  text-decoration: underline;
} */

.description-text::after {
  content: attr(title); /* Tooltip text from title attribute */
  position: absolute;
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 5px;
  border-radius: 5px;
  white-space: normal; /* Allow wrapping in tooltip */
  display: none;
}

.description-text:hover::after {
  display: block; /* Show tooltip on hover */
  top: 100%;
  left: 0;
  z-index: 10;
}
/* Badge Styling */
.meeting-badge {
  display: inline-block;
  padding: 5px 10px;
  text-align: center;
  border-radius: 10px;
  font-size: 0.8rem;
  margin-left: 4px;
  margin-top: 4px;
  font-weight: 500;
}

/* Green Badge (Today) */
.badge-green {
  background-color: #d4edda; /* Light green */
  color: #155724; /* Darker green */
}

/* Blue Badge (Tomorrow) */
.badge-blue {
  background-color: #cce5ff; /* Light blue */
  color: #004085; /* Darker blue */
}

/* Red Badge (No Contact) */
.badge-red {
  background-color: #f8d7da; /* Light red */
  color: #721c24; /* Darker red */
}

/* Grey Badge (Next Month / Others) */
.badge-grey {
  background-color: #e2e3e5; /* Light grey */
  color: #6c757d; /* Darker grey */
}

.scroll-visible {
  overflow-y: scroll; /* Enable scroll when necessary */
  scrollbar-width: thin; /* Thin scrollbar for modern browsers */
}

.scroll-visible::-webkit-scrollbar {
  width: 8px; /* Adjust scrollbar width */
}

.scroll-visible::-webkit-scrollbar-thumb {
  background-color: #aaa; /* Scrollbar thumb color */
  border-radius: 10px; /* Rounded scrollbar */
}

.scroll-visible::-webkit-scrollbar-thumb:hover {
  background-color: #888; /* Darker thumb on hover */
}

.members {
  display: flex;
  align-items: center;
  gap: 5px;
}

.member-avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  object-fit: cover;
}

.remaining-members {
  width: 30px;
  height: 30px;
  background-color: #e0e0e0;
  color: #6c757d;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  font-size: 0.85rem;
  font-weight: bold;
}
</style>
