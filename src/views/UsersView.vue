<template>
  <div>
    <h1>Users</h1>
    <p></p>
    <button @click="logout">Logout</button>
  </div>
  <div v-if="modifiedUsers.length" class="batch-panel">
    <p>One or more users have been modified.</p>
    <button @click="saveChanges">Save</button>
  </div>
  <div>
    <div className="header">Username</div>
    <div className="header status">Active</div>
  </div>
  <User v-for="(el, id) in users" :key="id" :user="el" :index="id" :currentUsername="currentUsername" :selectUser="selectUser" />

  <UserModal :closeModal="closeModal" :saveUser="saveUser" :selectedUser="selectedUser" :isModalOpen="isModalOpen" />
</template>

<script>
import config from "../../config";
import User from '../components/UserRow.vue'
import UserModal from '../components/UserModal.vue'
import axios from 'axios';

export default {
  components: {User, UserModal},
  data() {
    return {
      currentUsername: '',
      users: null,
      selectedUser: null,
      modifiedUsers: [],
      isModalOpen: false
    }
  },
  async beforeCreate() {
    // Check if JWT exists in localStorage
    const jwt = localStorage.getItem('jwt');
    if (!jwt) {
      this.$router.push('/login'); // Redirect to login page if no JWT
    }

    try {
      const response = await axios.get(`${config.apiBaseUrl}/User/GetAll`, {
        headers: {
          Authorization: `Bearer ${jwt}`,
        },
      }).then(res => (this.users = res.data));
      
      if(this.users == null){
        localStorage.removeItem('jwt');
        this.$router.push('/login'); // Redirect to login page after logout
      }
      
      this.currentUsername = localStorage.getItem('user-name')
    } catch (error) {
      if(error.status == 401){
        localStorage.removeItem('jwt');
        this.$router.push('/login'); // Redirect to login page after logout
      } else {
        console.error('Error fetching data:', error);
        throw error;
      }
    }
  },
  methods: {
    selectUser(user) {
      this.selectedUser = { ...user };
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
    },
    saveUser() {
      const originalUser = this.users.find((u) => u.id === this.selectedUser.id);
      if (originalUser && originalUser.isActive != this.selectedUser.isActive) {
        const existingModifiedUser = this.modifiedUsers.find((u) => u.id === this.selectedUser.id);
        if(existingModifiedUser && existingModifiedUser.oldState == this.selectedUser.isActive){
          const index = this.modifiedUsers.findIndex((u) => u.id === this.selectedUser.id)
          this.modifiedUsers.splice(index, 1)
        } else {
          this.modifiedUsers.push({"id":originalUser.id, "oldState":originalUser.isActive, "newState":this.selectedUser.isActive})
        }
        originalUser.isActive = this.selectedUser.isActive;
      }
      this.closeModal();
    },
    async saveChanges() {
      if (this.modifiedUsers.length === 0) {
        console.log("No changes to save.");
        return;
      }

      try {
        const jwt = localStorage.getItem('jwt');

        const response = await axios.put(
          `${config.apiBaseUrl}/User/Update`,
          this.modifiedUsers,
          {
            headers: {
              "Content-Type": "application/json",
              Authorization: `Bearer ${jwt}`
            },
          }
        ).then(res => (this.users = res.data));

        console.log("Changes saved successfully:", response.data);

        // Clear the modifiedUsers array after successful save
        this.modifiedUsers = [];
      } catch (error) {
        if(error.status == 401){
          localStorage.removeItem('jwt');
          this.$router.push('/login'); // Redirect to login page after logout
        } else {
          console.error('Error fetching data:', error);
          throw error;
        }
      }
    },
    logout() {
      localStorage.removeItem('jwt');
      this.$router.push('/login'); // Redirect to login page after logout
    },
  },
};
</script>

<style scoped>
.header {
  width: 200px;
  padding: 5px;
  margin: 3px;
  border-radius: 3px;
  border-bottom: 3px solid #000;
  background: #64626268;
  float: left;
  justify-content: space-around;
}

.status {
  width: 100px;
  text-align: center;
}

.batch-panel {
  background-color: #0b872b;
  padding: 10px;
  margin-bottom: 10px;
  height: 35px;
}

.batch-panel p {
  float: left;
  padding-top: 10px;
}

.batch-panel button {
  margin-top: 3px;
  float: right;
}
</style>