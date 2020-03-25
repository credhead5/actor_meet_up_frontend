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
        <h3 class="sidebarbox-title">Meeting with {{ meeting.partner.username }}</h3>
        <div class="sidebar-box">
          <ul class="list-unstyled cat-list">
            <li>
              <a href="#">{{ meeting.address }}</a>
            </li>
            <li>
              <a href="#">{{ formattedDate(meeting.start_time) }}</a>
            </li>
            <li>
              <a href="#">{{ formattedDate(meeting.end_time) }}</a>
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
      </div>
      <div class="divied-40"></div>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function() {
    return {
      meetings: [],
      errors: []
    };
  },
  created: function() {
    axios.get("/api/meetings").then(response => {
      this.meetings = response.data.sort((meeting1, meeting2) =>
        meeting1.start_time.localeCompare(meeting2.start_time)
      );
      console.log(response.data);
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
    formattedDate: function(date) {
      return moment(date).format("MMMM Do YYYY, h:mm a");
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
