<template>
    <div class="dropdown-container">
      <div class="dropdown">
        <button @click="showForm = !showForm">+ Create Invoice</button>
        <div v-if="showForm" class="dropdown-content">
          <form @submit.prevent="addData">
            <div>
              <label for="status">Status:</label>
              <select id="status" v-model="newUser.status" required>
              <option value="Delivered">Delivered</option>
              <option value="Pending">Pending</option>
              </select>
            </div>
            <div>
              <label for="name">Name:</label>
              <input type="text" id="name" v-model="newUser.name" required />
            </div>
            <div>
              <label for="email">Email:</label>
              <input type="email" id="email" v-model="newUser.email" required />
            </div>
            <div>
              <label for="total">Total:</label>
              <input type="number" id="total" v-model="newUser.total" required />
            </div>
            <div>
              <label for="issued_date">Issued Date:</label>
              <input type="date" id="issued_date" v-model="newUser.issued_date" required />
            </div>
            <div>
              <label for="balance">Balance:</label>
              <input type="number" id="balance" v-model="newUser.balance" required />
            </div>
            <div>
              <label for="id">ID:</label>
              <input type="text" id="id" v-model="newUser.id" required />
            </div>
            <button type="submit">Add Invoice</button>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        showForm: false,
        newUser: {
          status: 'Pending',
          name: '',
          email: '',
          total: '',
          issued_date: '',
          balance: '',
          id: ''
        }
      };
    },
    methods:{
      addData() {
  
        this.$emit('add', this.newUser);
        this.newUser = {
          status:'Pending',
          name: '',
          email: '',
          total: '',
          issued_date: '',
          balance: '',
          id: '',
        };
        this.showForm = false;
      }
    }
  };
  </script>
  
  <style scoped>
  .dropdown-container {
  position: relative;

}
  .dropdown {
    position: relative;
    display: inline-block;
  }
  
  .dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 200px;
    padding: 10px;
    z-index: 1;
    border: 1px solid #ccc; 
  border-radius: 5px; 
  }
  
  .dropdown-content form {
    display: flex;
    flex-direction: column;
  }
  
  .dropdown .dropdown-content {
    display: block;
  }
  button {
  background-color: #8e39bf;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 20px;
  margin-bottom: 20px;
}
label {
  margin-bottom: 5px;
}

input[type="text"],
input[type="email"],
input[type="number"],
input[type="date"],
select {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

  </style>
  