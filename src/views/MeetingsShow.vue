<template>
  <div class="meetings-show">
    <section id="page-tree">
      <div class="container">
        <div class="row">
          <div class="col-md-5 col-sm-5">
            <h1>Meeting Information</h1>
          </div>
        </div>
        <!--row-->
      </div>
      <!--container-->
    </section>

    {{ $parent.getUserId() }}

    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <div class="blog-items margin-btm40">
            <div id="map"></div>
            <h1><a href="blog-post.html" class="hover-color">Details</a></h1>
            <ul class="list-inline blog-post-list">
              <li>
                <i class="ion-location"></i>
                {{ meeting.address }}
              </li>
              <li>|</li>
              <li>
                <a href="#" class="hover-color">
                  <i class="ion-ios7-time"></i>
                  {{ meeting.start_time }}
                </a>
              </li>
              <li>-</li>
              <li>
                <a href="#" class="hover-color">
                  {{ meeting.end_time }}
                </a>
              </li>
            </ul>
            <div class="divied-40"></div>
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
                        <a type="submit" class="btn btn-dark">Edit</a>
                      </div>
                    </form>
                    <!--Contact form-->
                  </div>
                  <a v-on:click="destroyMeeting(meeting)" href="#" class="btn btn-dark">
                    Delete Meeting
                  </a>
                </div>
              </div>
            </div>
          </div>
          <!--blog items-->
        </div>
      </div>
    </div>
    <!--blog full width-->
  </div>
</template>

<style>
#map {
  top: 0;
  bottom: 0;
  width: 90%;
  height: 450px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      meeting: {},
      errors: []
    };
  },
  created: function() {
    axios.get("/api/meetings/18").then(response => {
      this.meeting = response.data;
      mapboxgl.accessToken =
        "pk.eyJ1IjoiY3JlZGhlYWQ1IiwiYSI6ImNrODBqeDA5NTBpOWIzbHJsOGl5eW91dnoifQ.kTS4_fLpxHgsUa13zrpGMw";
      var mapboxClient = mapboxSdk({ accessToken: mapboxgl.accessToken });
      mapboxClient.geocoding
        .forwardGeocode({
          query: this.meeting.address,
          autocomplete: false,
          limit: 1
        })
        .send()
        .then(function(response) {
          if (response && response.body && response.body.features && response.body.features.length) {
            var feature = response.body.features[0];

            var map = new mapboxgl.Map({
              container: "map",
              style: "mapbox://styles/mapbox/streets-v11",
              center: feature.center,
              zoom: 10
            });
            new mapboxgl.Marker().setLngLat(feature.center).addTo(map);
          }
        });
    });
  },
  methods: {
    submit: function() {
      var params = {
        start_time: this.meeting.start_time,
        end_time: this.meeting.end_time,
        address: this.meeting.address
      };
      axios
        .patch(`/api/meetings/${this.meeting.id}`, params)
        .then(response => {
          this.$router.push(`/meetings/${this.meeting.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyMeeting: function(meeting) {
      if (confirm("Are you sure you want to delete this meeting?")) {
        axios.delete(`/api/meetings/${meeting.id}`).then(response => {
          console.log("Sucessfully Deleted", response.data);
          this.$router.push("/meetings");
          var index = this.user.meetings.indexOf(meeting);
          this.user.meetings.splice(index, 1);
        });
      }
    }
  }

  // var mapboxClient = mapboxSdk({ accessToken: mapboxgl.accessToken });
  // mapboxClient.geocoding
  //   .forwardGeocode({
  //     query: '520 8th Avenute, NY',
  //     autocomplete: false,
  //     limit: 1
  //   })
  //   .send()
  //   .then(function(response) {
  //     if (
  //       response &&
  //       response.body &&
  //       response.body.features &&
  //       response.body.features.length
  //     ) {
  //       var feature = response.body.features[0];

  //       var map = new mapboxgl.Map({
  //         container: 'map',
  //         style: 'mapbox://styles/mapbox/streets-v11',
  //         center: feature.center,
  //         zoom: 10
  //       });
  //       new mapboxgl.Marker().setLngLat(feature.center).addTo(map);
  //     }
  //   });
};
</script>
