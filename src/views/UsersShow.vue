<template>
  <div class="users-show">
    <h1>Profile</h1>
    <h2>{{ user.username }}</h2>
    <h2>{{ user.email }}</h2>
    <h2>{{ user.bio }}</h2>
    <div v-for="review in user.reviews">
      <p>Reviews: {{ review.text }}</p>
    </div>

    <form v-on:submit.prevent="submit()">
      <h1>Edit Profile</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>User Name:</label>
        <input type="text" class="form-control" v-model="user.username" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="text" class="form-control" v-model="user.email" />
      </div>
      <div class="form-group">
        <label>Bio:</label>
        <input type="text" class="form-control" v-model="user.bio" />
      </div>
      <button type="submit">Edit</button>
    </form>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      user: {},
      errors: []
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      this.user = response.data;
      console.log(this.user);
    });
  },
  methods: {
    submit: function() {
      var params = {
        username: this.user.title,
        email: this.user.email,
        bio: this.user.bio
      };
      axios
        .patch(`/api/users/${this.user.id}`, params)
        .then(response => {
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
