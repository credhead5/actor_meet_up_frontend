<template>
  <div class="users-show">
    <h1>Profile</h1>
    <h2>{{ user.username }}</h2>
    <h2>{{ user.email }}</h2>
    <h2>{{ user.bio }}</h2>
    Reviews:
    <div v-for="review in user.reviews">
      <p>{{ review.evaluator_name }}</p>
      <p>{{ review.text }}</p>
      <button v-if="$parent.getUserId() == review.evaluator_id" v-on:click="destroyReview(review)">
        Delete Review
      </button>
    </div>

    <div>
      Text:
      <input type="text" v-model="newReviewText" />
      <br />

      <button v-on:click="createReview()">Create Review</button>
    </div>

    {{ $parent.getUserId() }}
    {{ user.id }}
    <div v-if="$parent.getUserId() == user.id">
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
      <button v-on:click="destroyUser()">Delete Profile</button>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      user: {},
      errors: [],
      newReviewText: ""
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
    },
    createReview: function() {
      var params = {
        actor_id: this.user.id,
        text: this.newReviewText
      };
      axios
        .post("/api/reviews", params)
        .then(response => {
          console.log(response.data);
          this.user.reviews.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    },
    destroyUser: function() {
      if (confirm("Are you sure you want to delete your profile?")) {
        axios.delete(`/api/users/${this.user.id}`).then(response => {
          console.log("Sucessfully Deleted", response.data);
          this.$router.push("/logout");
        });
      }
    },
    destroyReview: function(review) {
      if (confirm("Are you sure you want to delete this review?")) {
        axios.delete(`/api/reviews/${review.id}`).then(response => {
          console.log("Sucessfully Deleted", response.data);
          var index = this.user.reviews.indexOf(review);
          this.user.reviews.splice(index, 1);
        });
      }
    }
  }
};
</script>
