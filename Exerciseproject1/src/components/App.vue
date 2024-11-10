<template>
  <div class="container my-5">
    <!-- User Form to Add New User -->
    <div class="mb-4">
      <h3>{{ selectedUser ? 'Edit User' : 'Add New User' }}</h3>
      <form @submit.prevent="selectedUser ? updateUser() : addUser()">
        <div class="mb-3">
          <label for="name" class="form-label">Name</label>
          <input
            v-model="user.name"
            type="text"
            id="name"
            class="form-control custom-sm-input"
            placeholder="Enter Name"
            required
          />
        </div>
        <div class="mb-3">
          <label for="email" class="form-label">Email</label>
          <input
            v-model="user.email"
            type="email"
            id="email"
            class="form-control custom-sm-input"
            placeholder="Enter Email"
            required
          />
        </div>
        <div class="mb-3">
          <label for="phone" class="form-label">Phone</label>
          <input
            v-model="user.phone"
            type="text"
            id="phone"
            class="form-control custom-sm-input"
            placeholder="Enter Phone"
            required
          />
        </div>

        <!-- Add User Button aligned to the right -->
        <div class="d-flex justify-content-end">
          <button class="btn custom-sm-btn" type="submit">
            {{ selectedUser ? 'Update User' : 'Add User' }}
          </button>
        </div>
      </form>
    </div>

    <!-- User List -->
    <div v-for="user in users" :key="user.id" class="col-md-4 mb-3">
  <div class="card custom-card p-3">
    <h5 class="card-title">{{ user.name }}</h5>

    <!-- User Details and Action Inputs -->
    <div class="mb-3">
      <label for="user-email" class="form-label">Email</label>
      <input
        v-model="user.email"
        type="email"
        id="user-email"
        class="form-control custom-sm-input"
        placeholder="Edit Email"
      />
    </div>
    <div class="mb-3">
      <label for="user-phone" class="form-label">Phone</label>
      <input
        v-model="user.phone"
        type="text"
        id="user-phone"
        class="form-control custom-sm-input"
        placeholder="Edit Phone"
      />
    </div>

    <!-- Action Buttons Aligned Below Inputs -->
    <div class="d-grid gap-2 mt-3">
      <button class="btn btn-warning btn-sm custom-btn" @click="selectUser(user)">
        Edit
      </button>
      <button class="btn btn-danger btn-sm custom-btn" @click="deleteUser(user)">
        Delete
      </button>
      <button class="btn btn-info btn-sm custom-btn" @click="viewUser(user)">
        View
      </button>
    </div>
  </div>
</div>

    <!-- No Users Message -->
    <div v-if="users.length === 0" class="alert alert-info mt-4 custom-alert">
      No users available. Please add a user.
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  setup() {
    const user = ref({ name: "", email: "", phone: "" });
    const users = ref([]);
    const selectedUser = ref(null);

    const addUser = () => {
      const newUser = {
        id: Date.now(),
        name: user.value.name,
        email: user.value.email,
        phone: user.value.phone,
      };
      users.value.push(newUser);
      resetForm();
    };

    const selectUser = (userToEdit) => {
      selectedUser.value = userToEdit;
      user.value.name = userToEdit.name;
      user.value.email = userToEdit.email;
      user.value.phone = userToEdit.phone;
    };

    const updateUser = () => {
      const index = users.value.findIndex((u) => u.id === selectedUser.value.id);
      if (index !== -1) {
        users.value[index] = { ...selectedUser.value, ...user.value };
      }
      resetForm();
    };

    const deleteUser = (userToDelete) => {
      if (confirm(`Are you sure you want to delete ${userToDelete.name}?`)) {
        users.value = users.value.filter((u) => u.id !== userToDelete.id);
      }
    };

    const viewUser = (userToView) => {
      alert(`Viewing user: ${userToView.name}`);
    };

    const resetForm = () => {
      user.value.name = "";
      user.value.email = "";
      user.value.phone = "";
      selectedUser.value = null;
    };

    return {
      user,
      users,
      selectedUser,
      addUser,
      selectUser,
      updateUser,
      deleteUser,
      viewUser,
    };
  },
};
</script>

<style scoped>
/* Custom styles */
.container {
  background-color: #f3f6f9;
}

.custom-card {
  background-color: #e3f2fd; /* Light blue for card background */
  border-color: #ffffff; /* Custom border color */
}

.custom-sm-input {
  padding: 0.3rem 0.5rem;
  font-size: 0.975rem;
  max-width: 350px;
  border: 1px solid rgb(107, 99, 99);
  background-color: #ffffff; /* Light teal for input background */
}

.custom-sm-btn {
  padding: 0.3rem 0.5rem;
  font-size: 0.875rem;
  background-color: #e60e0e; /* Light blue for button */
  color: rgb(252, 247, 247);
  border: none;
  position: absolute;
  left: 420px;
}

.custom-btn {
  background-color: #ffffff; /* Matching light blue for action buttons */
  color: rgb(34, 32, 32);
  border: none;
  max-width: 100px;
}

.custom-btn:hover,
.custom-sm-btn:hover {
  background-color: #92d697; /* Darker blue on hover */
  font-family: Times, serif;
  font-size: 15px;
  margin-right: 500;

}

.custom-alert {
  background-color: hsl(0, 0%, 100%); /* Light yellow background */
  color: #141313; /* Dark yellow text */
  border-color: #ece5e5ef;
  font-family: Times, serif;
  font-size: 15px;
margin-right: 650px;  


}
</style>
