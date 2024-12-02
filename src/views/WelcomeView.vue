<template>
  <div>
    <h1>Welcome</h1>
    <p>Welcome, <b>{{userName}}</b>! This page is for authenticated users only.</p>
    <button @click="logout">Logout</button>
    <button @click="$router.push('/users')">User administration</button>
  </div>
</template>

<script>
import config from "../../config";
import axios from 'axios';

export default {
  data() {
    return {
      userInfo: null,
      userName: ''
    }
  },

  async beforeCreate() {
    // Check if JWT exists in localStorage
    const jwt = localStorage.getItem('jwt');
    if (!jwt) {
      this.$router.push('/login'); // Redirect to login page if no JWT
    }

    try {
      const response = await axios.get(`${config.apiBaseUrl}/User/Get`, {
        headers: {
          Authorization: `Bearer ${jwt}`,
        },
      }).then(res => (this.userInfo = res.data));
      
      if(this.userInfo != null && this.userInfo.isActive){
        this.userName = this.userInfo.username;
      } else {
        localStorage.removeItem('jwt');
        this.$router.push('/login'); // Redirect to login page after logout
      }
      
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
    logout() {
      localStorage.removeItem('jwt');
      this.$router.push('/login'); // Redirect to login page after logout
    },
  },
};
</script>

<style scoped>
button {
  float: left;
}
</style>