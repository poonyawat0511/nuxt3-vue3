<template>
  <v-container fluid>
    <div>
      <h1>Welcome to Activities Page</h1>
    </div>
    <v-row dense>
      <v-col
        v-for="activity in filteredActivities"
        :key="activity._id"
        cols="12"
        xl="2"
        md="3"
        sm="6"
        xs="12"
      >
        <ActivitiesCard
          :activity="activity"
          max-width="100%"
          rounded
          @click="openCard(activity._id)"
        ></ActivitiesCard>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const activities = ref<Activity[]>([]);
const isLoading = ref(true);

onMounted(async () => {
  try {
    const response = await fetch("http://localhost:8888/activity");
    activities.value = await response.json();
  } catch (error) {
    console.error("Error fetching activities:", error);
  } finally {
    isLoading.value = false;
  }
});

const openCard = (_id: number) => {
  router.push("/activities/" + _id);
};

const filteredActivities = computed(() => {
  return activities.value.filter(activity => activity.status === true);
});

definePageMeta({
  layout: "appbar",
});
</script>
