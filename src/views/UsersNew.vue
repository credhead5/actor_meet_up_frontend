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
      <input type="submit" class="btn btn-primary" value="Create User" />
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
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        username: this.username
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
