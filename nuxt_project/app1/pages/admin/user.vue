<script setup lang="ts">
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";

definePageMeta({
  layout: "appbaradmin",
});

const router = useRouter();
const search = ref("");
const Data = ref<User[]>([]);
const isLoading = ref(true);
const showFormDialog = ref(false); // Add this line to define the showFormDialog property

const formData = ref({
  id: "",
  name: "",
  description: "",
  score: "",
});

const headers = [
  { title: "STDid", value: "STDid" },
  { title: "Name", value: "name" },
  { title: "Email", value: "email" },
  { title: "Phone", value: "phone" },
  { title: "Actions", value: "actions" },
];

// Function to handle form submission for adding a new user
const handleSubmit = async () => {
  try {
    const response = await fetch("http://localhost:8888/user", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(formData.value),
    });

    if (response.ok) {
      const newUser = await response.json();
      Data.value.push(newUser);
      formData.value = {
        STDid: "",
        name: "",
        email: "",
        phone: "",
      };
      showFormDialog.value = false; // Close the dialog after submission
    } else {
      console.error("Failed to add user");
    }
  } catch (error) {
    console.error("Error submitting form:", error);
  }
};

// Function to fetch all users
const fetchData = async () => {
  try {
    const response = await fetch("http://localhost:8888/user");
    const data: User[] = await response.json();
    Data.value = data;
  } catch (error) {
    console.error("Error fetching data:", error);
  } finally {
    isLoading.value = false;
  }
};

// Function to delete an user by ID
const handleDelete = async (id: string) => {
  try {
    const response = await fetch(`http://localhost:8888/user/${id}`, {
      method: "DELETE",
    });

    console.log("Delete Response Status:", response.status);

    if (response.ok) {
      Data.value = Data.value.filter((user) => user._id !== id); // Update the filter condition
    } else {
      console.error("Failed to delete user");
    }
  } catch (error) {
    console.error("Error deleting user:", error);
  }
};

// Function to delete all users
const handleDeleteAll = async () => {
  try {
    const response = await fetch("http://localhost:8888/user", {
      method: "DELETE",
    });

    console.log("Delete Response Status:", response.status);

    if (response.ok) {
      // Clear the Data array to reflect the deletion on the frontend
      Data.value = [];
    } else {
      console.error("Failed to delete all users");
    }
  } catch (error) {
    console.error("Error deleting all users:", error);
  }
};

// Function to update an user by ID
const handleUpdate = async () => {
  const id = updateFormData.value._id; // Access _id property
  try {
    const response = await fetch(`http://localhost:8888/user/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(updateFormData.value),
    });

    if (response.ok) {
      // Update the user in the Data array with the updated details
      const updatedUser = await response.json();
      const index = Data.value.findIndex((user) => user._id === id);
      if (index !== -1) {
        Data.value[index] = updatedUser;
      }
      // Close the update dialog
      isUpdateDialogOpen.value = false;
    } else {
      console.error("Failed to update user");
    }
  } catch (error) {
    console.error("Error updating user:", error);
  }
};

// Function to handle update form submission
const handleUpdateSubmit = async () => {
  // Call the handleUpdate function to perform the update
  handleUpdate();
};

// Function to open the update dialog and populate the form with user data
const openUpdateDialog = (user: User) => {
  console.log("User data received:", user);
  updateFormData.value = { ...user };
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
  email: "",
  phone: "",
});
</script>


<template>
  <!-- vuetify table -->
  <v-card title="User Management" flat>
    <div v-if="isLoading">Loading...</div>
    <div v-else>
      <!-- Form for adding a new user inside a dialog -->
      <v-dialog v-model="showFormDialog" max-width="600px">
        <v-card>
          <v-card-title>
            Add User
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
                      v-model="formData.STDid"
                      label="STDid"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.name"
                      label="Name"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.email"
                      label="Email"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="formData.phone"
                      label="Phone"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-btn type="submit" color="primary">Add User</v-btn>
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
            <v-icon @click="handleDelete(item._id)" color="red">mdi-delete</v-icon>
          </v-col>
          <v-col>
            <v-icon icon @click="openUpdateDialog(item)">mdi-pencil</v-icon>
          </v-col>
        </template>
      </v-data-table>
      <div>
      <v-btn @click="handleDeleteAll" color="error"
        >Delete All User</v-btn
      >
      <!-- Button to open the form dialog -->
      <v-btn @click="showFormDialog = true" color="success">Add User</v-btn>
    </div>
    </div>
  </v-card>

  <v-dialog v-model="isUpdateDialogOpen" max-width="600px">
    <v-card>
      <v-card-title>Update User</v-card-title>
      <v-card-text>
        <v-form @submit.prevent="handleUpdateSubmit">
          <v-container>
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.STDid"
                  label="STDid"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.name"
                  label="Name"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.email"
                  label="Email"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="updateFormData.phone"
                  label="Phone"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-btn type="submit" color="primary" @click="handleUpdate"
              >Update User</v-btn
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
</style>
