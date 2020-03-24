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
          <a class="navbar-brand" href="index.html">
            <!-- <img src="/img/Actor_Meet_Up_3.png" alt="" /> -->
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

    <section id="footer-option" class="padding-60">
      <div class="container">
        <div class="row">
          <div class="col-md-4">
            <h3><img src="/img/Actor_Meet_Up_3.png" alt="" /></h3>
            <p>
              aliqua.adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna. aliqua.adipisicing
              elit, sed do eiusmod tempor incididunt ut labore et dolore magna.
            </p>
            <br />
            <ul class="list-unstyled list-inline social">
              <li>
                <a href="#">
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
                <a href="#">
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
          <div class="col-md-4">
            <h3>Recent Post</h3>
            <ul class="list-unstyled post-list">
              <li><a href="#" class="hover-color">Blog slide post</a></li>
              <li><a href="#" class="hover-color">Lorem ipsum dollor</a></li>
            </ul>
          </div>
          <div class="col-md-4">
            <h3>Newsletter</h3>
            <p>
              aliqua.adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna. aliqua.adipisicing
              elit
            </p>
            <br />
            <form>
              <div class="row">
                <div class="col-sm-12">
                  <div class="input-group">
                    <label class="sr-only" for="subscribe-email">Email address</label>
                    <input class="form-control" id="subscribe-email" placeholder="enter your mail id.." type="text" />
                    <span class="input-group-btn">
                      <button type="submit" class="btn btn-dark">OK</button>
                    </span>
                  </div>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </section>
    <!--footer option 2 end here-->
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
