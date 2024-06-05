<template>
  <v-app>
    <v-app-bar color="warning" dark>
      <v-app-bar-nav-icon @click="burger = !burger"></v-app-bar-nav-icon>
      <v-container class="d-flex align-center">
        <v-img
          src="C:\Users\poony\OneDrive\Desktop\nest-starter\nuxt_project\app1\public\logo.png"
          alt="Logo"
          max-height="50"
          contain
        ></v-img>
      </v-container>
    </v-app-bar>

    <v-navigation-drawer v-model="burger" app color="black">
      <!-- Add your navigation links or content here -->
      <v-list>
        <v-list-item link @click="navigateTo('/admin')">
          <v-list-item-icon>
            <v-icon>mdi-database-edit</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Activities Manage</v-list-item-title>
        </v-list-item>
        <v-list-item link @click="navigateTo('/admin/user')">
          <v-list-item-icon>
            <v-icon>mdi-account-edit</v-icon>
          </v-list-item-icon>
          <v-list-item-title>User Manage</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container>
        <slot></slot>
      </v-container>
    </v-main>
  </v-app>
</template>
    
    <script setup>
import { ref } from "vue";

const burger = ref(false);
</script>
    
    <style scoped>
.image {
  max-height: 50px;
}
</style>