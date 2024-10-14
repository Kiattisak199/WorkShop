<template>
    <v-app>
      <v-app-bar app color="brown-lighten-1">
        <v-btn icon @click="toggleSidebar">
          <v-icon>mdi-menu</v-icon>
        </v-btn>
        <v-toolbar-title class="text-h5 font-weight-medium">Admin Management</v-toolbar-title>
      </v-app-bar>
  
      <v-navigation-drawer app v-model="sidebar" color="brown-lighten-4" permanent>
        <v-list>
          <v-list-item @click="$router.push('/')">
            <div class="d-flex">
              <v-list-item-icon class="mr-2">
                <v-icon class="text-h5">mdi-home</v-icon> <!-- Home icon -->
              </v-list-item-icon>
              <v-list-item-title class="font-weight-medium text-h6">Home</v-list-item-title>
            </div>
          </v-list-item>
          <v-list-item @click="$router.push('/products')">
            <div class="d-flex">
              <v-list-item-icon class="mr-2">
                <v-icon class="text-h5">mdi-package-variant</v-icon> <!-- Products icon -->
              </v-list-item-icon>
              <v-list-item-title class="font-weight-medium text-h6">Products</v-list-item-title>
            </div>
          </v-list-item>
          <!-- Add more navigation items with icons as needed -->
        </v-list>
      </v-navigation-drawer>
  
      <v-main>
        <NuxtPage />
      </v-main>
    </v-app>
  </template>
  
  <script setup>
  definePageMeta({
    layout: 'default' // Ensure to use the layout where the sidebar is defined
  });
  const sidebar = ref(false); // State for controlling sidebar visibility
  
  const toggleSidebar = () => {
    sidebar.value = !sidebar.value; // Toggle sidebar
  };
  </script>
  