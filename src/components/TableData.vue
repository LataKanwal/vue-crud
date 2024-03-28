<template>
    <div class="search-container">
    <table class="user-table">
      <thead>
        <tr>
          <th class="checkbox-col"> SELECT </th>
          <th>#</th>
          <th>STATUS</th>
          <th>CLIENT</th>
          <th>TOTAL</th>
          <th>ISSUED DATE</th>
          <th>BALANCE</th>
          <th>ACTIONS</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in paginatedUsers" :key="user.id">
          <td><input type="checkbox" v-model="selectedUsers[index]"></td>
          <td>#{{ user.id }}</td>
          <td><img :src="getStatusIcon(user.status)" alt="Status Icon" class="status-icon" /></td>
          <td class="client-col">
            <div>
              <img :src="user.user_image" alt="User Image" class="user-image" />
              <span>{{ user.name }}</span>
            </div>
            <div>{{ user.email }}</div>
          </td>
          <td>${{ user.total }}</td>
          <td>{{ user.issued_date }}</td>
          <td>
            <img v-if="user.balance == 0" src="@/assets/paid.png" alt="Paid Icon" class="paid-icon" />
            <span v-else>$-{{ user.balance }}</span>
          </td>
          <td class="action-icons">
            <img src="@/assets/edit.png" alt="Edit Icon" @click="editUser(user.id)" class="edit-icon" />
            <img src="@/assets/view.png" alt="View Icon" @click="viewUser(user)" class="view-icon" />
            <img src="@/assets/delete.png" alt="Delete Icon" @click="deleteUser(user.id)" class="delete-icon" />
          </td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1" class="page-button">Previous</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages" class="page-button">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    users: {
      type: Array,
      required: true,
    },
    view: {
      type: Function,
      required: true,
    },
    pageSize: {
      type: Number,
      default: 10,
    },
  },
  data() {
    return {
      currentPage: 1,
      selectedUsers: [],
  
    };
  },
  computed: {
    totalUsers() {
      return this.users.length;
    },
    totalPages() {
      return Math.ceil(this.totalUsers / this.pageSize);
    },
    paginatedUsers() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = Math.min(startIndex + this.pageSize - 1, this.totalUsers - 1);
      return this.users.slice(startIndex, endIndex + 1);
    },
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    deleteUser(userId) {
      this.$emit('delete', userId);
    },
    getStatusIcon(status) {
      const iconMap = {
        Delivered: require('@/assets/delivered.png'),
        Pending: require('@/assets/pending.png'),
      };
      return iconMap[status] || '';
    },
    editUser(userId) {
      console.log('Editing user with ID:', userId);
      this.$emit('edit', userId);
    },
    viewUser(user) {
      console.log('Viewing user with ID:', user);
      this.$emit('view', user);
    },
  },
};
</script>

<style scoped>

.search-container {
  margin-bottom: 10px;
  position: relative;
}

.user-image {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}

.user-table .client-col span {
  font-weight: bold;
}

.user-table {
  width: 100%;
  border-collapse: collapse;
}

.invoice-search input[type="text"] {
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

table {
  width: 100%;
  border-collapse: collapse;
}

.user-table th,
.user-table td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.user-table th {
  background-color: #f2f2f2;
  font-weight: bold;
}

.user-table .checkbox-col {
  width: 80px;
}

.user-table .client-col div {
  margin-bottom: 4px;
}

.status-icon {
  width: 20px;
  height: auto;
}

.user-table tbody tr:hover {
  background-color: #f5f5f5;
}

.user-table button:focus {
  outline: none;
}

.pagination {
  margin-top: 10px;
  text-align: center;
}

.pagination button {
  margin: 0 5px;
}

.page-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 8px 16px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  cursor: pointer;
  border-radius: 4px;
}

.page-button:hover {
  background-color: #45a049;
}
.paid-icon {
  width: 40px;
  height: auto;
}

.user-table .action-icons {
  display: flex;
  justify-content: space-between;
  width: 100px;
}

.edit-icon,
.view-icon,
.delete-icon {
  width: 25px;
  height: 25px;
  cursor: pointer;
}
</style>
