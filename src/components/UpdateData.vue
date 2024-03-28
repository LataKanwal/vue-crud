<template>
  <div class="update-data-popup">
    <div class="update-data-container">
      <div class="update-data-content">
        <h2>Edit User Data</h2>
        <form @submit.prevent="submitForm">
          <div>
            <label for="status">Status:</label>
              <select id="status" v-model="updatedUser.status" required>
              <option value="Delivered">Delivered</option>
              <option value="Pending">Pending</option>
              </select>
              </div>
              <div>
            <label for="name">Name:</label>
            <input type="text" id="name" v-model="updatedUser.name" required>
          </div>
              <div>
            <label for="email">Email:</label>
            <input type="email" id="email" v-model="updatedUser.email" required>
          </div>
          <div>
            <label for="total">Total:</label>
            <input type="number" id="total" v-model="updatedUser.total" required>
          </div>
          <div>
            <label for="issued_date">Issued Date:</label>
            <input type="date" id="issued_date" v-model="updatedUser.issued_date" required>
          </div>
          <div>
              <label for="balance">Balance:</label>
              <input type="number" id="balance" v-model="updatedUser.balance" required />
            </div>
          
          
          <div class="button-group">
            <button type="submit" class="update-button">Update User</button>
            <button type="button" @click="closeUpdateData" class="cancel-button">Cancel</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      updatedUser: { ...this.user },
    };
  },
  methods: {
    submitForm() {
      this.$emit('update', this.updatedUser);
      this.closeUpdateData();
    },
    closeUpdateData() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped>
.update-data-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.update-data-container {
  background-color: #fff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.update-data-content {
  max-width: 400px;
  margin: 0 auto;
}

.update-data-content h2 {
  margin-bottom: 20px;
}

.update-data-content form div {
  margin-bottom: 15px;
}

.update-data-content form div label {
  display: block;
  margin-bottom: 5px;
}

.update-data-content form div input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.button-group {
  display: flex;
  justify-content: space-between;
}

.update-button,
.cancel-button {
  padding: 8px 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.update-button {
  background-color: #4caf50;
  color: #fff;
}

.cancel-button {
  background-color: #f44336;
  color: #fff;
}
</style>
