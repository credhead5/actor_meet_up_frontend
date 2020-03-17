<template>
  <div class="meetings-index">
    <h1>All Meetings</h1>
    <div v-for="meeting in meetings">
      <h2>{{ meeting.location }}</h2>
      <h2>{{ meeting.start_time }}</h2>
      <h2>{{ meeting.end_time }}</h2>
      <h2>{{ meeting.partner.username }}</h2>
      <h2>{{ meeting.partner.email }}</h2>
      <form v-on:submit.prevent="submit()">
        <h1>Edit Meeting</h1>
        <ul>
          <li v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Seeker:</label>
          <input type="text" class="form-control" v-model="meeting.seeker_id" />
        </div>
        <div class="form-group">
          <label>Fulfiller:</label>
          <input type="text" class="form-control" v-model="meeting.fulfiller_id" />
        </div>
        <div class="form-group">
          <label>Location:</label>
          <input type="text" class="form-control" v-model="meeting.location" />
        </div>
        <button type="submit">Edit</button>
      </form>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      meetings: [],
      errors: []
    };
  },
  created: function() {
    axios.get("/api/meetings").then(response => {
      this.meetings = response.data;
      console.log(this.meetings);
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
