<template>
  <div class="users-show">
    <h1>Profile</h1>
    <h2>{{ user.username }}</h2>
    <h4>{{ user.email }}</h4>
    <h5>{{ user.bio }}</h5>
    <img :src="user.image_url" alt="" />

    <h2>Reviews:</h2>
    <div v-for="review in user.reviews">
      <p>{{ review.evaluator_name }}</p>
      <p>{{ review.text }}</p>
      <button v-if="$parent.getUserId() == review.evaluator_id" v-on:click="destroyReview(review)">
        Delete Review
      </button>
    </div>

    <div v-if="$parent.getUserId() != user.id">
      <div>
        <h2>New Review</h2>
        Text:
        <input type="text" v-model="newReviewText" />
        <br />

        <button v-on:click="createReview()">Create Review</button>
      </div>
      <div>
        <h2>New Meeting</h2>
        Address:
        <input type="text" v-model="newMeetingAddress" />
        <br />
        Start Time:
        <input type="text" v-model="newMeetingStartTime" />
        <br />
        End Time:
        <input type="text" v-model="newMeetingEndTime" />
        <br />

        <button v-on:click="createMeeting()">Create Meeting</button>
      </div>
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
        <div class="form-group">
          <label>Image:</label>
          <input type="" class="form-control" v-model="user.image_url" />
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
      newReviewText: "",
      newMeetingAddress: "",
      newMeetingStartTime: "",
      newMeetingEndTime: ""
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
        bio: this.user.bio,
        image_url: this.user.image_url
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
    createMeeting: function() {
      var params = {
        fulfiller_id: this.user.id,
        Address: this.newMeetingAddress,
        start_time: this.newMeetingStartTime,
        end_time: this.newMeetingEndTime
      };
      axios
        .post("/api/meetings", params)
        .then(response => {
          console.log(response.data);
          this.user.meetings.push(response.data);
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
