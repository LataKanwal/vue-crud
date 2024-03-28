<template>
  <div class="parent-component">
    <div class="card">
      <div>
        <AddData @add="addUser" />
        <TableData :users="users" @delete="deleteUser" @edit="editUser" @view="viewUser" />
        <UpdateData v-if="selectedUserId" :user="selectedUser" @update="updateUser" @close="closeModal" />
        <ViewData v-if="viewedUser" :user="viewedUser" @close="closeModal" />
      </div>
    </div>
  </div>
</template>

<script>
import AddData from './AddData.vue';
import TableData from './TableData.vue';
import UpdateData from './UpdateData.vue';
import ViewData from './ViewData.vue'; 
import axios from 'axios';
import { API_ENDPOINT } from './apiConfig.js';

export default {
  components: {
    AddData,
    TableData,
    UpdateData,
    ViewData, 
  },
  data() {
    return {
      selectedUser: null,
      selectedUserId: null,
      viewedUser: null,
      users: [],
    };
  },
  created() {
    this.fetchUsers();
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await axios.get(API_ENDPOINT);
        this.users = response.data;
      } catch (error) {
        console.error('Error fetching users:', error);
      }
    },
    async addUser(newUser) {
      try {
        const response = await axios.post(API_ENDPOINT, newUser);
        this.users.push(response.data);
        console.log('User added:', response.data);
      } catch (error) {
        console.error('Error adding user:', error);
      }
    },
    async deleteUser(userId) {
      try {
        await axios.delete(`${API_ENDPOINT}/${userId}`);
        this.users = this.users.filter(user => user.id !== userId);
        console.log('User deleted with ID:', userId);
      } catch (error) {
        console.error('Error deleting user:', error);
      }
    },
    editUser(userId) {
      console.log('Editing user with ID:', userId);
      this.selectedUserId = userId;
      this.selectedUser = this.users.find(user => user.id === userId);
    },
    async updateUser(updatedUser) {
      try {
        const response = await axios.put(`${API_ENDPOINT}/${updatedUser.id}`, updatedUser);
        const index = this.users.findIndex(user => user.id === updatedUser.id);
        if (index !== -1) {
          this.$set(this.users, index, response.data);
        }
        console.log('User updated:', response.data);
      } catch (error) {
        console.error('Error updating user:', error);
      } finally {
        this.fetchUsers();
      }
    },
    viewUser(user) {
      this.viewedUser = user;
    },
    closeModal() {
      this.selectedUserId = null;
      this.viewedUser = null;
    },
  },
};
</script>

<style scoped>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
}
</style>
