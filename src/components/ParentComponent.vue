<template>
  <div class="card">
    <div>
      <AddData @add="addUser"/>
      <TableData :users="users" @delete="deleteUser" />
    </div>
  </div>
  </template>
  
  <script>
  import AddData from './AddData.vue';
  import TableData from './TableData.vue';
  import axios from 'axios';
  import { API_ENDPOINT } from './apiConfig.js';

  export default {
    components: {
      TableData,
      AddData,
    },
    data() {
      return {
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
    this.users.push(response.data); // Assuming the API returns the added user data
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
      
    },
  };
  </script>

<style>
.page-container {
  display:flex;
  flex-direction: column;
  align-items: center;
}

.create-button {
    background-color: rgb(169, 59, 233); 
    color:white;
    padding : 10px;
    font-size: 18px;
    margin: 20px;
    border:none;
    text-decoration: none;
    border-radius: 8px;

  }
  .add {
  margin-bottom: 20px;
  }
  .card {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
}
  </style>
  