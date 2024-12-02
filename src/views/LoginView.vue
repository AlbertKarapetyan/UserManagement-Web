<template>
  <div>
    <h1>Login</h1>
    <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    <form @submit.prevent="login">
      <p><label for="username">Username:</label>
        <input type="text" v-model="username" placeholder="Login" required />
      </p>
      <p>
        <label for="password">Password:</label>
        <input type="password" v-model="password" placeholder="Password" required />
      </p>
      <p>
        <button type="submit">Login</button>
      </p>
    </form>
  </div>
</template>

<script>
import config from "../../config";

export default {
  data() {
    return {
      username: '',
      password: '',
      errorMessage: '',
    };
  },
  methods: {
    async login() {
      try {
        this.errorMessage = ""
        const response = await fetch(`${config.apiBaseUrl}/Identity/Authenticate`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            username: this.username,
            password: this.password,
          }),
        });

        if (response.ok) {
          const token = await response.text()
          if(token != '') {
            // If login is successful, store JWT in localStorage
            localStorage.setItem('jwt', token);
            localStorage.setItem('user-name', this.username);
            this.$router.push('/welcome'); // Redirect to welcome page
          } else {
            // If login fails, show error message
            this.password = '';
            this.errorMessage = 'We could not log you in. Please check your username/password and try again.';
          }
        } if (response.status === 401) {
          this.password = '';
          this.errorMessage = 'Unauthorized user, the user is not active!';
        } else {
          this.password = '';
          this.errorMessage = 'We could not log you in. Please check your username/password and try again.';
        } 
      } catch (error) {
        // Handle network errors
        this.password = '';
        this.errorMessage = 'An error occurred. Please try again later.';
      }
    },
  },
};
</script>
