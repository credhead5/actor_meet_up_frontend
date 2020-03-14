<template>
  <div class="users-new">
    <h1>New Users</h1>
    <form v-on:submit.prevent="submit()">
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Username:</label>
        <input type="text" class="form-control" v-model="username" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" class="form-control" v-model="email" />
        <div class="form-group">
          <label>Bio:</label>
          <input type="text" class="form-control" v-model="bio" />
        </div>
        <div class="form-group">
          <label>Password:</label>
          <input type="password" class="form-control" v-model="password" />
        </div>
      </div>
      <input type="submit" value="Create User" />
    </form>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      username: "",
      email: "",
      bio: "",
      password: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        username: this.username,
        email: this.email,
        bio: this.bio,
        password: this.password
      };
      axios
        .post("/api/users", params)
        .then(respose => {
          this.$rouster.push("/users");
        })
        .catch(error => {
          this.errors = error.respose.data.errors;
        });
    }
  }
};
</script>
