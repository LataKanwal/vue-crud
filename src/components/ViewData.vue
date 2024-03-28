<template>
  <div class="modal">
    <div class="modal-content">
      <span class="close" @click="closeModal">&times;</span>
      <div class="user-details" v-if="user">
        <h2>User Details</h2>
        <div class="details-item">
          <label>ID:</label>
          <span>{{ user.id }}</span>
        </div>
        <div class="details-item">
          <label>Status:</label>
          <span>{{ user.status}}</span>
        </div>
        <div class="details-item">
          <label>Name:</label>
          <span>{{ user.name }}</span>
        </div>
        <div class="details-item">
          <label>Email:</label>
          <span>{{ user.email }}</span>
        </div>
        <div class="details-item">
          <label>Total:</label>
          <span>{{ user.total }}</span>
        </div>
        <div class="details-item">
          <label>Issued Date:</label>
          <span>{{ user.issued_date }}</span>
        </div>
        <div class="details-item">
          <label>Balance:</label>
          <span>{{ user.balance }}</span>
        </div>
      </div>
      <div v-else>
        <p>No user data available.</p>
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
  methods: {
    closeModal() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: #f9fafc;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  max-width: 600px;
  width: 90%;
}

.close {
  color: #333; 
  float: right;
  font-size: 24px;
  font-weight: bold;
  cursor: pointer;
}

.close:hover {
  color: #000;
}

.user-details {
  margin-top: 20px;
}

.details-item {
  margin-bottom: 15px;
}

label {
  font-weight: bold;
  margin-right: 10px;
  color: #1f0404; 
}

span {
  font-size: 16px;
  color: #333; 
}

h2 {
  font-size: 24px;
  margin-bottom: 20px;
  color: #065141; 
}
</style>
