<script setup lang="ts">
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";

definePageMeta({
  layout: "appbaradmin",
});

const router = useRouter();
const search = ref("");
const Data = ref<Activity[]>([]);
const isLoading = ref(true);
const showFormDialog = ref(false); // Add this line to define the showFormDialog property

const formData = ref({
  id: "",
  name: "",
  description: "",
  status: "null",
  score: "",
  picture: "",
});
const statusOptions = [
  { title: "Active", value: true },
  { title: "Inactive", value: false },
];

const headers = [
  { title: "ID", value: "id" },
  { title: "Name", value: "name" },
  { title: "Description", value: "description" },
  { title: "Status", value: "status" },
  { title: "Score", value: "score" },
  { title: "Picture", value: "picture" },
  { title: "Actions", value: "actions" },
];

// Function to handle form submission for adding a new activity
const handleSubmit = async () => {
  try {
    const response = await fetch("http://localhost:8888/activity", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(formData.value),
    });

    if (response.ok) {
      const newActivity = await response.json();
      Data.value.push(newActivity);
      formData.value = {
        id: "",
        name: "",
        description: "",
        score: "",
        picture: "",
      };
      showFormDialog.value = false; // Close the dialog after submission
    } else {
      console.error("Failed to add activity");
    }
  } catch (error) {
    console.error("Error submitting form:", error);
  }
};

// Function to fetch all activities
const fetchData = async () => {
  try {
    const response = await fetch("http://localhost:8888/activity");
    const data: Activity[] = await response.json();
    Data.value = data;
  } catch (error) {
    console.error("Error fetching data:", error);
  } finally {
    isLoading.value = false;
  }
};

// Function to delete an activity by ID
const handleDelete = async (id: string) => {
  try {
    const response = await fetch(`http://localhost:8888/activity/${id}`, {
      method: "DELETE",
    });

    console.log("Delete Response Status:", response.status);

    if (response.ok) {
      Data.value = Data.value.filter((activity) => activity._id !== id); // Update the filter condition
    } else {
      console.error("Failed to delete activity");
    }
  } catch (error) {
    console.error("Error deleting activity:", error);
  }
};

// Function to delete all activities
const handleDeleteAll = async () => {
  try {
    const response = await fetch("http://localhost:8888/activity", {
      method: "DELETE",
    });

    console.log("Delete Response Status:", response.status);

    if (response.ok) {
      // Clear the Data array to reflect the deletion on the frontend
      Data.value = [];
    } else {
      console.error("Failed to delete all activities");
    }
  } catch (error) {
    console.error("Error deleting all activities:", error);
  }
};

// Function to update an activity by ID
const handleUpdate = async () => {
  const id = updateFormData.value._id; // Access _id property
  try {
    const response = await fetch(`http://localhost:8888/activity/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(updateFormData.value),
    });

    if (response.ok) {
      // Update the activity in the Data array with the updated details
      const updatedActivity = await response.json();
      const index = Data.value.findIndex((activity) => activity._id === id);
      if (index !== -1) {
        Data.value[index] = updatedActivity;
      }
      // Close the update dialog
      isUpdateDialogOpen.value = false;
    } else {
      console.error("Failed to update activity");
    }
  } catch (error) {
    console.error("Error updating activity:", error);
  }
};

// Function to handle update form submission
const handleUpdateSubmit = async () => {
  // Call the handleUpdate function to perform the update
  handleUpdate();
};

// Function to open the update dialog and populate the form with activity data
const openUpdateDialog = (activity: Activity) => {
  console.log("Activity data received:", activity);
  updateFormData.value = { ...activity };
  isUpdateDialogOpen.value = true;
};

//mount

onMounted(() => {
  fetchData();
});

const isUpdateDialogOpen = ref(false);
const updateFormData = ref({
  id: "null",
  name: "",
  description: "",
  status: "",
  score: "",
  picture: "",
});
</script>


<template>
  <!-- vuetify table -->
  <v-card title="Activity Management" flat>
    <div v-if="isLoading">Loading...</div>
    <div v-else>
      <!-- Form for adding a new activity inside a dialog -->
      <v-dialog v-model="showFormDialog" max-width="600px">
        <v-card>
          <v-card-title>
            Add Activity
            <v-spacer></v-spacer>
            <v-btn icon @click="showFormDialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text>
            <v-form @submit.prevent="handleSubmit">
              <v-container>
                <v-row>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.id"
                      label="ID"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.name"
                      label="Activity Name"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.description"
                      label="Description"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-select
                      v-model="formData.status"
                      :items="statusOptions"
                      label="Status"
                      required
                    ></v-select>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.score"
                      label="Score"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.picture"
                      label="Url Picture"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-btn type="submit" color="primary">Add Activity</v-btn>
              </v-container>
            </v-form>
          </v-card-text>
        </v-card>
      </v-dialog>

      <v-data-table :headers="headers" :items="Data" :search="search">
        <template #top>
          <v-text-field
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            single-line
            hide-details
          ></v-text-field>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-col>
            <v-icon @click="handleDelete(item._id)" color="red"
              >mdi-delete</v-icon
            >
          </v-col>
          <v-col>
            <v-icon @click="openUpdateDialog(item)">mdi-pencil</v-icon>
          </v-col>
        </template>
      </v-data-table>
      <div>
        <v-btn @click="handleDeleteAll" color="error"
          >Delete All Activities</v-btn
        >
        <!-- Button to open the form dialog -->
        <v-btn class="addbtn" @click="showFormDialog = true" color="success"
          >Add Activity</v-btn
        >
      </div>
    </div>
  </v-card>

  <!-- update form -->
  <v-dialog v-model="isUpdateDialogOpen" max-width="600px">
    <v-card>
      <v-card-title>Update Activity</v-card-title>
      <v-card-text>
        <v-form @submit.prevent="handleUpdateSubmit">
          <v-container>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.id"
                  label="ID"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.name"
                  label="Activity Name"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.description"
                  label="Description"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-select
                  v-model="updateFormData.status"
                  :items="statusOptions"
                  label="Status"
                  required
                ></v-select>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.score"
                  label="Score"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.picture"
                  label="Picture"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-btn type="submit" color="primary" @click="handleUpdate"
              >Update Activity</v-btn
            >
          </v-container>
        </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<style scoped>
/* Add your styles here */
.container {
  background-color: black;
}
.addbtn {
}
</style>
