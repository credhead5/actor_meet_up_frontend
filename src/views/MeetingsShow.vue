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
      meeting: {}
    };
  },
  created: function() {
    axios.get("/api/meetings/2").then(response => {
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
