<template>
  <div id="app">
    <ParentComponent :users="users" @edit="editUser" @delete="deleteUser" />
  </div>
</template>

<script>
import ParentComponent from './components/ParentComponent.vue';

export default {
  name: 'App',
  components: {
    ParentComponent,
  },
  data() {
    return {
      users: [
      ],
    };
  },
  methods: {
    editUser(user) {
  console.log('Editing user:', user);
    },
    deleteUser(userId) {
      console.log('Deleting user with ID:', userId);
      this.users = this.users.filter(user => user.id !== userId);
    },
  },
};
</script>