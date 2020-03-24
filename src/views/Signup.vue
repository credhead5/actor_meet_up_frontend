<template>
  <div class="signup">
    <section id="page-tree">
      <div class="container">
        <div class="row">
          <div class="col-md-5 col-sm-5">
            <h1>Signup</h1>
          </div>
        </div>
        <!--row-->
      </div>
      <!--container-->
    </section>
    <div class="divied-60"></div>
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <form v-on:submit.prevent="submit()" class="margin-btm40">
            <ul>
              <li v-for="error in errors">{{ error }}</li>
            </ul>
            <div class="row">
              <div class="col-sm-6">
                <div class="form-group">
                  <label>Username:</label>
                  <input type="text" class="form-control" v-model="username" />
                </div>
              </div>
              <div class="col-sm-6">
                <div class="form-group">
                  <label>Email:</label>
                  <input type="email" class="form-control" v-model="email" />
                </div>
              </div>
              <div class="col-sm-12">
                <div class="form-group">
                  <label>Bio:</label>
                  <textarea class="form-control" v-model="bio" rows="4"></textarea>
                </div>
              </div>
              <div class="col-sm-12">
                <div class="form-group">
                  <label>Password:</label>
                  <input type="password" class="form-control" v-model="password" />
                </div>
              </div>
              <div class="col-sm-12">
                <div class="form-group">
                  <label>Password confirmation:</label>
                  <input type="password" class="form-control" v-model="passwordConfirmation" />
                </div>
              </div>

              <div class="col-sm-12 text-right">
                <button type="submit" class="btn btn-dark btn-lg">Create Profile</button>
              </div>
            </div>
          </form>
          <!--Contact form-->
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
      username: "",
      email: "",
      bio: "",
      password: "",
      passwordConfirmation: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        username: this.username,
        email: this.email,
        bio: this.bio,
        password: this.password,
        password_confirmation: this.passwordConfirmation
      };
      axios
        .post("/api/users", params)
        .then(respose => {
          this.$rouster.push("/users");
        })
        .catch(error => {
          this.errors = error.respose.data.errors;
        });
    }
  }
};
</script>
