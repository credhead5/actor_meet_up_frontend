<template>
  <div class="meetings-index">
    <section id="page-tree">
      <div class="container">
        <div class="row">
          <div class="col-md-5 col-sm-5">
            <h1>My Meetings</h1>
          </div>
        </div>
        <!--row-->
      </div>
      <!--container-->
    </section>
    <!--page-tree-->
    <div class="divied-60"></div>

    <h1>My Meetings</h1>

    <div v-for="meeting in meetings">
      <h2>{{ meeting.address }}</h2>
      <h2>{{ meeting.start_time }}</h2>
      <h2>{{ meeting.end_time }}</h2>
      <h2>{{ meeting.partner.username }}</h2>
      <h2>{{ meeting.partner.email }}</h2>
      <router-link :to="`/meetings/${meeting.id}`">See meeting details</router-link>
      <button v-on:click="destroyMeeting(meeting)">
        Delete Meeting
      </button>
      <form v-on:submit.prevent="submit()">
        <h1>Edit Meeting</h1>
        <ul>
          <li v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Start Time:</label>
          <input type="text" class="form-control" v-model="meeting.start_time" />
        </div>
        <div class="form-group">
          <label>End Time:</label>
          <input type="text" class="form-control" v-model="meeting.end_time" />
        </div>
        <div class="form-group">
          <label>Address:</label>
          <input type="text" class="form-control" v-model="meeting.address" />
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
    destroyMeeting: function(meeting) {
      if (confirm("Are you sure you want to delete this meeting?")) {
        axios.delete(`/api/meetings/${meeting.id}`).then(response => {
          console.log("Sucessfully Deleted", response.data);
          var index = this.user.meetings.indexOf(meeting);
          this.user.meetings.splice(index, 1);
        });
      }
    }
  }
};
</script>
