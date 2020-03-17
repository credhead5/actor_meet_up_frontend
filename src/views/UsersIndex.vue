<template>
  <div class="users-index">
    <h1>All Users</h1>
    <div v-for="user in users">
      <h2>{{ user.username }}</h2>
      <h2>{{ user.bio }}</h2>
      <router-link :to="`/users/${user.id}`">More Info</router-link>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      users: []
    };
  },
  created: function() {
    axios.get("/api/users").then(response => {
      this.users = response.data;
      console.log(this.users);
    });
  },
  methods: {}
};
</script>
