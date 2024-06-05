<template>
  <v-container class="profile-container" fluid>
    <v-card class="profile-card" max-width="600">
      <v-card-title class="profile-header">
        <v-row align="center" justify="space-between" class="w-100">
          <div class="profile-title">
            <v-avatar color="primary" size="65" class="mr-4">
            </v-avatar>
            <span class="profile-title-text">{{
              profileData.name || "Profile"
            }}</span>
          </div>
          <v-btn color="warning" @click="editProfile" v-if="!isEditing">
            <v-icon>mdi-pen</v-icon>
          </v-btn>
        </v-row>
      </v-card-title>
      <v-divider></v-divider>
      <v-card-subtitle class="profile-subtitle">
        User Information
      </v-card-subtitle>
      <v-card-text>
        <v-form>
          <v-row>
            <v-col cols="12" md="6">
              <v-text-field
                v-model="profileData.STDid"
                label="Student ID"
                :disabled="!isEditing"
                outlined
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                v-model="profileData.name"
                label="Name"
                :disabled="!isEditing"
                outlined
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                v-model="profileData.email"
                label="Email"
                :disabled="!isEditing"
                outlined
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="6">
              <v-text-field
                v-model="profileData.phone"
                label="Phone"
                :disabled="!isEditing"
                outlined
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row justify="end" v-if="isEditing">
            <v-btn color="primary" class="mr-4" @click="saveProfile">
              Save
            </v-btn>
            <v-btn color="secondary" @click="cancelEdit"> Cancel </v-btn>
          </v-row>
        </v-form>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";
definePageMeta({
  layout: "appbar",
});

const isEditing = ref(false);
const profileData = ref({
  STDid: "",
  name: "",
  email: "",
  phone: "",
});

const editProfile = () => {
  isEditing.value = true;
};

const saveProfile = () => {
  // Add your save logic here
  isEditing.value = false;
  // Update profile title text
  document.querySelector(".profile-title-text").textContent =
    profileData.value.name;
};

const cancelEdit = () => {
  // Add your cancel logic here (e.g., revert changes)
  isEditing.value = false;
};
</script>

<style scoped>
.profile-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.profile-card {
  width: 100%;
  max-width: 600px;
  margin-top: 30px;
}

.profile-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.profile-title {
  display: flex;
  align-items: center;
}

.profile-title-text {
  font-size: 1.5rem;
  font-weight: bold;
}

.profile-subtitle {
  margin-top: 10px;
  font-size: 1rem;
  color: #666;
}

.author-profile img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
.w-100 {
  height: 100px;
}
</style>
