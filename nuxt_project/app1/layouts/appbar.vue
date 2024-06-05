<template>
  <v-app>
    <v-app-bar color="#FFC100" dark>
      <v-app-bar-nav-icon @click="Ham = !Ham"></v-app-bar-nav-icon>
      <v-container class="d-flex align-center">
        <v-img
          src="C:\Users\poony\OneDrive\Desktop\nest-starter\nuxt_project\app1\public\logo.png"
          alt="Logo"
          max-height="50"
          contain
        ></v-img>
      </v-container>
    </v-app-bar>

    <v-navigation-drawer v-model="Ham" app color="#C40C0C">
      <!-- Add your navigation links or content here -->
      <v-list>
        <v-list-item link @click="navigateTo('/')">
          <v-list-item-icon>
            <v-icon>mdi-home</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Home</v-list-item-title>
        </v-list-item>
        <v-list-item link @click="navigateTo('/profile')">
          <v-list-item-icon>
            <v-icon>mdi-account</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Profile</v-list-item-title>
        </v-list-item>
        <v-list-item link @click="navigateTo('/#')">
          <v-list-item-icon>
            <v-icon>mdi-calendar-badge</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Calendar</v-list-item-title>
        </v-list-item>
        <a href="https://www.facebook.com/messages/t/114668883550855">
          <v-list-item link>
            <v-list-item-icon>
              <v-icon>mdi-help-circle</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Help</v-list-item-title>
          </v-list-item>
        </a>
        <v-list-item link @click="navigateTo('/activities')">
          <v-list-item-icon>
            <v-icon>mdi-chart-timeline-variant-shimmer</v-icon>
          </v-list-item-icon>
          <v-list-item-title>Activity</v-list-item-title>
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

const Ham = ref(false);
</script>
  
  <style scoped>
.image {
  max-height: 50px;
}

a:link {
  text-decoration: none;

  color: white;
}
</style>
  