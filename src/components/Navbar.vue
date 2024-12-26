<template>
  <div>
    <!-- Hamburger Menu -->
    <div class="menu-icon-container">
      <button id="menu-toggle" class="btn btn-light" @click="toggleNavbar">
        <span class="navbar-toggler-icon"></span>
      </button>
    </div>

    <div v-show="isNavbarVisible" class="sidebar">
      <!-- Header Section -->
      <div class="header">
        <img src="@/assets/logo.jpg" alt="Logo" class="logo" />
        <span class="company-name">INK <br />InnovateHub</span>
        <img src="@/assets/user.jpg" alt="User" class="user-icon" />
      </div>
      <hr class="divider" />

      <!-- Menu Group Section -->
      <div class="menu-group">
        <div class="menu-item">
          <img src="@/assets/design.png" alt="Design Team" class="menu-icon" />
          <span class="menu-text">Design Team</span>
          <span class="badge">+1</span>
        </div>
        <hr class="divider" />

        <div class="menu-item">
          <img
            src="@/assets/marketing.png"
            alt="Marketing Team"
            class="menu-icon"
          />
          <span class="menu-text">Marketing Team</span>
          <span class="badge">+2</span>
        </div>
        <hr class="divider" />

        <div class="menu-item">
          <img
            src="@/assets/development.png"
            alt="Development Team"
            class="menu-icon"
          />
          <span class="menu-text">Development Team</span>
          <span class="badge">+3</span>
        </div>
        <hr class="divider" />

        <div class="menu-item create-team">
          <img src="@/assets/add.png" alt="Create Team" class="menu-icon" />
          <span class="menu-text">Create a Team</span>
        </div>
        <hr class="divider" />
      </div>

      <!-- Folders Section -->
      <div class="folders-section">
        <div class="folders-title">
          <span class="menu-text">FOLDERS</span>
          <span class="add-icon">+</span>
        </div>
        <hr class="divider" />

        <div v-for="folder in folders" :key="folder.name">
          <!-- Folder Item -->
          <div class="folder-item" @click="$emit('menuSelected', folder.name)">
            <img
              src="@/assets/folder.png"
              alt="Folder Icon"
              class="menu-icon"
            />
            <span class="menu-text">{{ folder.name }}</span>
            <span
              v-if="folder.subItems && folder.subItems.length > 0"
              @click.stop="toggleFolder(folder)"
              class="caret-icon"
            >
              {{ folder.expanded ? "▲" : "▼" }}
            </span>
          </div>

          <!-- Sub-items (Visible if Expanded) -->
          <ul v-show="folder.expanded" class="sub-items">
            <li v-for="subItem in folder.subItems" :key="subItem">
              <div class="sub-item">{{ subItem }}</div>
            </li>
            <li class="add-new" @click="addSubItem(folder)">+ Add new sub</li>
          </ul>
          <hr class="divider" />
        </div>
      </div>

      <!-- Bottom Section -->
      <div class="bottom-section">
        <div class="menu-item">
          <img
            src="@/assets/add-user.png"
            alt="Invite Teammates"
            class="menu-icon"
          />
          <span class="menu-text">Invite teammates</span>
        </div>
        <hr class="divider" />

        <div class="menu-item">
          <img src="@/assets/help.png" alt="Help" class="menu-icon" />
          <span class="menu-text">Help and first steps</span>
          <span class="badge">0/6</span>
        </div>
        <hr class="divider" />

        <div class="menu-item">
          <span class="menu-text">
            <span class="badge">{{ trialDays }}</span> days left on trial
          </span>
          <button class="btn btn-sm add_billing">Add billing</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Nav-bar",
  data() {
    return {
      isNavbarVisible: true,
      trialDays: 7, // Dynamic trial days
      folders: [
        {
          name: "ProductsView",
          expanded: false,
          subItems: ["Roadmap", "Feedback", "Performance", "Team", "Analytics"],
        },
        {
          name: "Sales",
          expanded: false,
          subItems: ["Revenue", "Clients"],
        },
        {
          name: "Design",
          expanded: false,
          subItems: ["Mockups", "Prototypes"],
        },
        { name: "Office", expanded: false, subItems: [] },
        { name: "Legal", expanded: false, subItems: [] },
      ],
    };
  },
  methods: {
    toggleFolder(folder) {
      folder.expanded = !folder.expanded;
    },
    addSubItem(folder) {
      folder.subItems.push("New Sub-Item");
    },
    toggleNavbar() {
      this.isNavbarVisible = !this.isNavbarVisible;
    },
  },
};
</script>
