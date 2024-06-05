<template>
  <v-container fluid>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6">
        <v-card v-if="!isLoading && activity" class="mt-5">
          <v-img
            :src="activity.picture || 'https://via.placeholder.com/400x200'"
            height="10"
            class="fill-height"
            :alt="activity.name"
          ></v-img>
          <v-card-title class="headline">{{ activity.name }}</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <p><strong>Description:</strong> {{ activity.description }}</p>

            <p><strong>Score:</strong> {{ activity.score }}</p>
          </v-card-text>
          <div class="startbtn">
          <v-btn color="warning">Let's Begin</v-btn>
          </div>
        </v-card>
        <v-alert v-else-if="isLoading" type="info" class="mt-5"
          >Loading...</v-alert
        >
        <v-alert v-else type="error" class="mt-5"
          >No data found for ID: {{ activityId }}</v-alert
        >
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const activityId = route.params.id;

const activity = ref<Activity | null>(null);
const isLoading = ref(true);

interface Activity {
  name: string;
  description: string;
  status: boolean;
  score: number;
  picture?: string;
}

onMounted(async () => {
  try {
    const response = await fetch(
      `http://localhost:8888/activity/${activityId}`
    );
    activity.value = await response.json();
  } catch (error) {
    console.error("Error fetching activity:", error);
  } finally {
    isLoading.value = false;
  }
});

definePageMeta({
  layout: "appbar",
});
</script>

<style scoped>
.headline {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 16px;
}
.fill-height {
  object-fit: cover;
}
.startbtn {
  display: flex;
  justify-content: end;
  padding: 10px;
}
</style>
