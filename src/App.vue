<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
      |
      <router-link to="/about">About</router-link>
      |
      <router-link v-if="!isLoggedIn()" class="nav-item" to="/signup">Signup</router-link>

      <router-link v-if="!isLoggedIn()" class="nav-item" to="/login">Login</router-link>

      <router-link v-if="isLoggedIn()" class="nav-item" to="/logout">Logout</router-link>
      |
      <router-link to="/users">All Users</router-link>
      |
      <router-link to="/users/new">New Users</router-link>
      |
      <router-link :to="`/users/${user_id}`">My Account</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    }
  }
};
</script>
