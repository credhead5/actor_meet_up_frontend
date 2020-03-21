<template>
  <div class="meetings-show">
    <h1>Meeting Profile</h1>
    <h2>{{ meeting.address }}</h2>
    <h2>{{ meeting.start_time }}</h2>
    <h2>{{ meeting.end_time }}</h2>

    {{ $parent.getUserId() }}
    <div id="map"></div>
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
    axios.get("/api/meetings/1").then(response => {
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
