<template>
  <div class="users-show">
    <section id="page-tree">
      <div class="container">
        <div class="row">
          <div class="col-md-5 col-sm-5">
            <h1>{{ user.username }}'s Profile</h1>
          </div>
        </div>
        <!--row-->
      </div>
      <!--container-->
    </section>
    <!--page-tree-->
    <div class="divied-60"></div>

    <div class="container">
      <div class="row">
        <div class="col-md-7">
          <img :src="user.image_url" alt="" />
        </div>
        <div class="col-md-5">
          <h3>Description</h3>
          <p>
            {{ user.bio }}
          </p>

          <div class="divied-40"></div>
          <div class="sidebar-box port-single-desc">
            <p>
              <strong>Email:</strong>
              {{ user.email }}
            </p>
          </div>
        </div>
      </div>
      <!--portfolio single description row end-->

      <div class="divied-40"></div>
      <div class="row">
        <div class="col-md-12">
          <h3>
            Reviews
            <i class="ion-ios7-chatbubble"></i>
          </h3>
        </div>

        <div v-for="review in user.reviews" class="col-md-12 margin-btm20">
          <div class="comment">
            <i class="ion-person"></i>
            <h4>
              {{ review.evaluator_name }} |
              <span>
                14 may 2014 |
                <a
                  v-if="$parent.getUserId() == review.evaluator_id"
                  v-on:click="destroyReview(review)"
                  class="hover-color"
                >
                  Delete
                </a>
              </span>
            </h4>
            <p>
              {{ review.text }}
            </p>
          </div>
        </div>
        <!--comment-->
      </div>
      <!--comment row-->
    </div>
    <!--page container end-->
    <div class="divied-60"></div>
    <div v-if="$parent.getUserId() != user.id" class="container">
      <div class="row">
        <div class="col-md-8">
          <div class="form-box margin-btm40">
            <form>
              <h3>New Review</h3>
              <div class="row">
                <div class="col-sm-12">
                  <div class="form-group">
                    <label>Text:</label>
                    <textarea type="text" v-model="newReviewText" class="form-control" rows="3"></textarea>
                  </div>
                </div>
                <div class="col-sm-12 text-right">
                  <button v-on:click="createReview()">Create Review</button>
                </div>
              </div>
            </form>
            <!--Contact form-->
          </div>
        </div>
      </div>

      <div class="container">
        <div class="row">
          <div class="col-md-8">
            <div class="form-box margin-btm40">
              <form>
                <h3>New Meeting</h3>
                <div class="row">
                  <div class="col-sm-6">
                    <div class="form-group">
                      <label>Address:</label>
                      <input type="text" v-model="newMeetingAddress" class="form-control" />
                    </div>
                  </div>
                  <div class="col-sm-6">
                    <div class="form-group">
                      <label>Start Time:</label>
                      <input type="email" v-model="newMeetingStartTime" class="form-control" />
                    </div>
                  </div>
                  <div class="col-sm-12">
                    <div class="form-group">
                      <label>End Time:</label>
                      <input type="text" v-model="newMeetingEndTime" class="form-control" />
                    </div>
                  </div>
                </div>
                <div class="sidebar-box  port-single-desc">
                  <a v-on:click="createMeeting()" href="#" class="btn btn-dark">Schedule a meeting</a>
                </div>
              </form>
              <!--Contact form-->
            </div>
          </div>
        </div>
      </div>
    </div>

    {{ $parent.getUserId() }}
    {{ user.id }}
    <div v-if="$parent.getUserId() == user.id" class="container">
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
                    <label>User Name:</label>
                    <input type="text" class="form-control" v-model="user.username" />
                  </div>
                </div>
                <div class="col-sm-6">
                  <div class="form-group">
                    <label>Email:</label>
                    <input type="email" v-model="user.email" class="form-control" />
                  </div>
                </div>
                <div class="col-sm-12">
                  <div class="form-group">
                    <label>Bio:</label>
                    <input type="text" v-model="user.bio" class="form-control" />
                  </div>
                </div>
                <div class="col-sm-12">
                  <div class="form-group">
                    <label>Image:</label>
                    <input type="text" v-model="user.image_url" class="form-control" />
                  </div>
                </div>
              </div>
              <div class="sidebar-box  port-single-desc">
                <a v-on:click="createMeeting()" href="#" class="btn btn-dark">Schedule a meeting</a>
              </div>
            </form>
            <!--Contact form-->
          </div>
        </div>
      </div>
    </div>

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
        address: this.newMeetingAddress,
        start_time: this.newMeetingStartTime,
        end_time: this.newMeetingEndTime
      };
      axios
        .post("/api/meetings", params)
        .then(response => {
          console.log(response.data);
          this.user.meetings.push(response.data);
          this.$router.push("/meetings");
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
