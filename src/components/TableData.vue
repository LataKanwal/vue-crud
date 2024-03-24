<template>
    <div class="invoice-search">
      <div class="search container">
    <input type="text" v-model="searchQuery" placeholder="Search Invoice..." />
  </div>
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
          <td><img :src="getStatusIcon(getStatus(user.status))" alt="Status Icon" class="status-icon" /></td>
          <td class="client-col">
            <div>
              <img :src="user.user_image" alt="User Image" class="user-image" />
              <span>{{ user.name }}</span>
            </div>
            <div>{{ user.email }}</div>
          </td>
          <td>${{ user.total }}</td>
          <td>{{ user.issued_date }}</td>
       <td> <img v-if="user.balance == 0" src="@/assets/paid.png" alt="Paid Icon" class="paid-icon" />
        <span v-else>${{ user.balance }}</span></td>
          <td> <img src="@/assets/delete.png" alt="Delete Icon" @click="remove(user.id)" class="delete-icon" /></td>
          
        </tr>
        <tr v-for="invoice in filteredInvoices" :key="invoice.id">
         
        </tr>

      </tbody>
    </table>
     <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
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
    pageSize: {
      type: Number,
      default: 10, 
    },
  },
  data() {
    return {
      currentPage: 1,
      selectedUsers: [],
      searchQuery:'',
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
    remove(userId) {
      this.$emit('delete', userId);
    },
    getStatus(status) {
      return status ? 'Delivered' : 'Pending';
    },
    getStatusIcon(status) {
      const iconMap = {
        Delivered: require('@/assets/delivered.png'),
        Pending: require('@/assets/pending.png'),
      };
      return iconMap[status] || '';
    }
  },
};
</script>

<style scoped>

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
  border-spacing: 0;
}
.invoice-search input[type="text"] {
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  position: relative;
}
.search-container {
  margin-bottom: 10px;
  position: absolute;
  margin-left: 20px;
  top: 0;
  left: 0;
  padding: 10px;
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

.user-table button {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 3px;
}

.user-table button:hover {
  background-color: #c82333;
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
.paid-icon {
  width: 40px;
  height: auto;
}
.user-table .delete-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
}
.delete-icon {
  width: 20px;
  height: auto;
  cursor: pointer; 
}
</style>
