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

    <div v-for="meeting in meetings">
      <div class="container">
        <h3 class="sidebarbox-title">My Meetings</h3>
        <div class="sidebar-box">
          <ul class="list-unstyled cat-list">
            <li>
              <a href="#">{{ meeting.address }}</a>
            </li>
            <li>
              <a href="#">{{ meeting.start_time }}</a>
            </li>
            <li>
              <a href="#">{{ meeting.end_time }}</a>
            </li>
            <li>
              <a href="#">{{ meeting.partner.username }}</a>
            </li>
            <li>
              <a href="#">{{ meeting.partner.email }}</a>
            </li>
          </ul>
        </div>
        <router-link :to="`/meetings/${meeting.id}`" class="btn btn-warning">See meeting details</router-link>
        <div class="divied-35"></div>
        <a v-on:click="destroyMeeting(meeting)" href="#" class="btn btn-dark">Delete Meeting</a>
      </div>
      <div class="divied-60"></div>
      <div class="container">
        <div class="row">
          <div class="col-md-8">
            <div class="form-box margin-btm40">
              <form v-on:submit.prevent="submit()">
                <h3>Edit Profile</h3>
                <ul>
                  <li class="text-danger" v-for="error in errors">{{ error }}</li>
                </ul>
                <div class="row">
                  <div class="col-sm-6">
                    <div class="form-group">
                      <label>Start Time:</label>
                      <input type="text" class="form-control" v-model="meeting.start_time" />
                    </div>
                  </div>
                  <div class="col-sm-6">
                    <div class="form-group">
                      <label>End Time:</label>
                      <input type="email" v-model="meeting.end_time" class="form-control" />
                    </div>
                  </div>
                  <div class="col-sm-12">
                    <div class="form-group">
                      <label>Address:</label>
                      <input type="text" v-model="meeting.address" class="form-control" />
                    </div>
                  </div>
                </div>
                <div class="sidebar-box  port-single-desc">
                  <button type="submit" class="btn btn-dark">Edit</button>
                </div>
              </form>
              <!--Contact form-->
            </div>
          </div>
        </div>
      </div>
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
