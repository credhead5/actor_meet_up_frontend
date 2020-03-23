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

    <h1>All Users</h1>
    <div>
      SEARCH:
      <input type="text" v-model="bioFilter" />
    </div>
    <div v-for="user in filterBy(users, bioFilter, 'bio')">
      <h2>{{ user.username }}</h2>
      <h2>{{ user.bio }}</h2>
      <img :src="user.image_url" alt="" />
      <router-link :to="`/users/${user.id}`">More Info</router-link>
    </div>
  </div>
</template>

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
