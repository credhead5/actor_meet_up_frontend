<template>
  <div class="users-show">
    <h1>Profile</h1>
    <h2>{{ user.username }}</h2>
    <h2>{{ user.email }}</h2>
    <h2>{{ user.bio }}</h2>
    <div v-for="review in user.reviews">
      <p>{{ review.text }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      user: {}
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      this.user = response.data;
      console.log(this.user);
    });
  },
  methods: {}
};
</script>
