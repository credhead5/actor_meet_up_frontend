<template>
  <div class="users-index">
    <section id="page-tree">
      <div class="container">
        <div class="row">
          <div class="col-md-5 col-sm-5">
            <h1>All Actors</h1>
          </div>
        </div>
        <!--row-->
      </div>
      <!--container-->
    </section>
    <!--page-tree-->
    <div class="divied-60"></div>

    <div class="container">
      <div>
        <h3 class="sidebarbox-title">
          Search:

          <input type="text" v-model="bioFilter" />
        </h3>
      </div>
      <div class="row">
        <div v-for="user in filterBy(users, bioFilter, 'bio')" class="col-md-4 margin-btm20">
          <div class="blog-grid-box">
            <div class="image-sec">
              <img :src="user.image_url" alt="" />
              <div class="image-overlay">
                <p><i class="ion-ios7-plus-empty"></i></p>
              </div>
            </div>
            <!--image-->

            <div class="blog-grid-desc">
              <h4 href="blog-post.html" class="hover-color">{{ user.username }}</h4>

              <p>
                {{ user.bio }}
              </p>
              <p class="more-button">
                <router-link :to="`/users/${user.id}`" class="btn btn-xs btn-dark">More Info</router-link>
              </p>
              <div class="blog-grid-btm"></div>
            </div>
          </div>
        </div>
      </div>
      <!--row-->
    </div>
    <!--container-->
  </div>
</template>

<style>
img {
  width: 300px;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      users: [],
      bioFilter: ""
    };
  },
  created: function() {
    axios.get("/api/users").then(response => {
      this.users = response.data;
      console.log(this.users);
    });
  },
  methods: {}
};
</script>
