<template>
  <div id="app">
    <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
      <div class="container">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="/users">
            <img src="/img/Actor_Meet.png" alt="" class="img-responsive" />
          </a>
        </div>
        <div class="navbar-collapse collapse">
          <ul class="nav navbar-nav navbar-right">
            <li>
              <router-link to="/users">
                Actors
              </router-link>
            </li>
            <li v-if="isLoggedIn()">
              <router-link to="/meetings">
                My Meetings
              </router-link>
            </li>
            <li v-if="isLoggedIn()">
              <router-link :to="`/users/${getUserId()}`">
                My Account
              </router-link>
            </li>
            <li v-if="isLoggedIn()">
              <router-link to="/logout">
                Logout
              </router-link>
            </li>
            <li v-if="!isLoggedIn()">
              <router-link to="/login">
                Login
              </router-link>
            </li>
            <li v-if="!isLoggedIn()">
              <router-link to="/signup">
                Signup
              </router-link>
            </li>
          </ul>
        </div>

        <!--/.nav-collapse -->
      </div>
      <!--/.container-->
    </div>
    <!--navigation end-->
    <router-view :key="$route.params.id" />
    <div class="divied-60"></div>

    <section id="footer-option" class="padding-40">
      <div class="container text-center">
        <div class="row">
          <div class="col-md-12">
            <br />
            <ul class="list-unstyled list-inline social">
              <li>
                <a href="http://www.facebook.com">
                  <i
                    class="ion-social-facebook-outline"
                    data-toggle="tooltip"
                    data-placement="top"
                    title=""
                    data-original-title="Like On Facebook"
                  ></i>
                </a>
              </li>
              <li>
                <a href="https://twitter.com/ActorsUp">
                  <i
                    class="ion-social-twitter-outline"
                    data-toggle="tooltip"
                    data-placement="top"
                    title=""
                    data-original-title="Follow On twitter"
                  ></i>
                </a>
              </li>
              <li>
                <a href="#">
                  <i
                    class="ion-social-googleplus-outline"
                    data-toggle="tooltip"
                    data-placement="top"
                    title=""
                    data-original-title="Follow On googleplus"
                  ></i>
                </a>
              </li>
              <li>
                <a href="#">
                  <i
                    class="ion-social-pinterest-outline"
                    data-toggle="tooltip"
                    data-placement="top"
                    title=""
                    data-original-title="pinterest"
                  ></i>
                </a>
              </li>
              <li>
                <a href="#">
                  <i
                    class="ion-social-skype-outline"
                    data-toggle="tooltip"
                    data-placement="top"
                    title=""
                    data-original-title="skype"
                  ></i>
                </a>
              </li>
            </ul>
          </div>
          <div class="col-md-1"></div>
        </div>
      </div>
    </section>
    <!--footer option 2 end here-->
    <section id="footer-copyright">
      <div class="container text-center">
        <span>&copy; Copyright 2014.all right reserved theme by design_mylife</span>
      </div>
    </section>
    <!--footer copyright-->
  </div>
</template>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    }
  }
};
</script>
