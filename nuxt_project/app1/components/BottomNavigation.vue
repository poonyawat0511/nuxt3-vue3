<template>
  <v-layout
    class="overflow-visible"
    style="height: 56px; position: fixed; bottom: 0; width: 100%; z-index: 100"
  >
    <v-bottom-navigation v-model="value" :bg-color="color" mode="shift">
      <v-btn @click="navigateTo('/')">
        <v-icon>mdi-home</v-icon>
        <span>Home</span>
      </v-btn>
      <v-btn @click="navigateTo('/activities')">
        <v-icon>mdi-mouse</v-icon>
        <span>Music</span>
      </v-btn>
      <v-btn @click="navigateTo('/#')">
        <v-icon>mdi-book</v-icon>
        <span>Book</span>
      </v-btn>
      <v-btn @click="navigateTo('/contactAdmin')">
        <v-icon>mdi-image</v-icon>
        <span>Image</span>
      </v-btn>
    </v-bottom-navigation>
  </v-layout>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const value = ref(0);
const color = ref("$teal-300");

const navigateTo = (path: string) => {
  router.push({ path });
};
</script>

<style scoped>
/* Add your styles here */
</style>
