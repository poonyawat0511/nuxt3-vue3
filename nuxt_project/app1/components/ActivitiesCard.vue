<template>
  <v-card class="mx-auto my-5" max-width="300" @click="handleClick">
    <v-img
      :src="activity.picture || defaultPicture"
      alt="Activity Image"
      height="200px"
    ></v-img>
    <v-card-title>{{ activity.name }}</v-card-title>
    <v-card-subtitle>ID: {{ activity.id }}</v-card-subtitle>
    <v-card-text>
      <p><strong>Description:</strong> {{ activity.description }}</p>
      <p><strong>Status:</strong> {{ getStatusLabel }}</p>

      <p><strong>Score:</strong> {{ activity.score }}</p>
    </v-card-text>
    <v-card-actions class="d-flex justify-end">
      <v-btn color="primary">Info</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script setup lang="ts">
import { defineProps } from "vue";
import { useRouter } from "vue-router";

// Define the props
const props = defineProps<{
  activity: {
    id: number;
    name: string;
    description: string;
    status: boolean;
    score: number;
    picture?: string; // Make picture field optional
  };
}>();

const router = useRouter();

// const defaultPicture = "https://picsum.photos/400/300"; // Define a default picture URL

const handleClick = () => {
  router.push(`/activity/${props.activity.id}`);
};

const getStatusLabel = computed(() => {
  return props.activity.status ? "Active" : "Inactive";
});
</script>

<style scoped>
/* Add any custom styles here */
.v-card {
  cursor: pointer;
}
</style>
