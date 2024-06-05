<script setup lang="ts">
import { useRouter } from "vue-router";

definePageMeta({
  layout: "appbar",
});

const router = useRouter();

const navigateTo = (path: string) => {
  router.push({ path });
};
</script>

<template>
  <v-container fluid class="container">
    <v-row dense>
      <v-col cols="12" md="12">
        <DashInfo max-width="" rounded="xl"></DashInfo>
      </v-col>
    </v-row>
  </v-container>
</template>
<style scoped>
/* Add your styles here */
.container {
  background-color: black;
}
</style>
