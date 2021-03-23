<template>
  <div id="app">
    <nav id="topNav" class="navbar navbar-default mb-3 navbar-expand-lg fixed-top" data-toggle="affix">
      <!-- this toggler only displays on offcanvas layout -->
      <a
        href="#"
        class="navbar-toggler-anim toggler-offcanvas my-auto collapsed border-0 mr-2"
        data-toggle="collapse"
        data-target="#offcanvasNavbar"
      >
        <span></span>
        <span></span>
        <span></span>
      </a>
      <!-- text brand or logo image goes here -->
      <a class="navbar-brand page-scroll text-uppercase wide-space link" href="./">gonatural</a>
      <button
        class="navbar-toggler collapsed border-0"
        type="button"
        data-toggle="collapse"
        data-target="#collapsingNavbar"
      >
        <span></span>
        <span></span>
        <span></span>
      </button>
      <div class="collapse navbar-collapse border-0" id="collapsingNavbar">
        <ul class="nav navbar-nav">
          <li class="nav-item">
            <a class="nav-link page-scroll" data-target="#one" href="./#one">Cards</a>
          </li>
          <li class="nav-item">
            <a class="nav-link page-scroll" data-target="#two" href="./#two">Flexbox</a>
          </li>
          <li class="nav-item">
            <a class="nav-link page-scroll" data-target="#three" href="./#three">Content</a>
          </li>
          <li class="nav-item">
            <a class="nav-link page-scroll" data-target="#blog" href="./blog.html">Blog</a>
          </li>
          <li class="nav-item">
            <a class="nav-link page-scroll" data-target="#five" href="./#five">Masonry</a>
          </li>
          <li class="nav-item">
            <a class="nav-link page-scroll" data-target="#last" href="./#last">More</a>
          </li>
        </ul>
        <ul class="nav navbar-nav ml-auto">
          <li class="nav-item dropdown dropdown-hover megamenu">
            <a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">
              Welcome
              <span class="caret"></span>
            </a>
            <div class="dropdown-menu dropdown-menu-right pt-1 pb-2">
              <div class="row mx-1">
                <div class="col-12">
                  <span class="close d-sm-none" data-toggle="dropdown">&#xD7;</span>
                </div>
                <ul class="col-md-6 col-6 list-unstyled small" role="menu"></ul>
                <ul class="col-md-6 col-6 list-unstyled small text-right" role="menu">
                  <li class="text-uppercase text-muted text-wide pt-3 pb-1">User</li>

                  <!-- Signup</router-link> -->

                  <li><a class="link" v-if="!isLoggedIn()" href="/login">Sign-up | Login</a></li>
                  <li><a class="link" v-if="isLoggedIn()" href="/logout">Logout</a></li>
                  <div v-if="isLoggedIn()">
                    <li class="text-uppercase text-muted text-wide pt-3 pb-1">Profile</li>
                    <li><a class="link" :href="`/users/${getUserId()}`">View/Edit</a></li>
                  </div>
                  <div v-if="isLoggedIn()">
                    <li class="text-uppercase text-muted text-wide pt-3 pb-1">Create</li>

                    <li><a class="link" href="/posts/new">New post</a></li>
                  </div>
                </ul>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </nav>

    <router-view />

    <footer id="footer" class="pt-5">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
            <p class="display-2 mb-2">Okay, Boss.</p>
          </div>
        </div>
        <div class="row">
          <div class="col-6 col-md-3">
            <h6 class="text-uppercase">Information</h6>
            <ul class="list-unstyled">
              <li><a href="./products.html">Products</a></li>
              <li><a href="./faq.html">FAQ&apos;s</a></li>
              <li><a href="./portfolio.html">Gallery</a></li>
              <li><a href="./clients.html">Clients</a></li>
            </ul>
          </div>
          <div class="col-6 col-md-3 column">
            <h6 class="text-uppercase">About</h6>
            <ul class="list-unstyled">
              <li><a href="./contact.html">Contact Us</a></li>
              <li><a href="./about.html">Our Mission</a></li>
              <li><a href="./legal.html">Privacy Policy</a></li>
              <li><a href="./legal.html">Terms &amp; Conditions</a></li>
            </ul>
          </div>
          <div class="col-12 col-md-3 column">
            <h6 class="text-uppercase">Stay Posted</h6>
            <form>
              <div class="form-group">
                <input type="text" class="form-control" title="Enter your email. No spam, we promise!" placeholder="" />
              </div>
              <div class="form-group">
                <button
                  class="btn btn-primary text-uppercase small"
                  data-toggle="modal"
                  data-target="#alertModal"
                  type="button"
                >
                  Subscribe
                </button>
              </div>
            </form>
          </div>
          <div class="col-12 col-lg-2 ml-lg-auto col-md-3 ml-md-0 text-right">
            <h6 class="text-uppercase">Jobs</h6>
            <p>
              <em><a href="">We&apos;re looking for great talent. Browse the job openings.</a></em>
            </p>
          </div>
          <div class="col-12 text-md-right text-center">
            <br />
            <ul class="list-inline">
              <li class="list-inline-item">
                <a rel="nofollow" href="//twitter.com/ThemesGuide" title="Twitter">
                  <i class="ion-social-twitter-outline"></i>
                </a>
                &#xA0;
              </li>
              <li class="list-inline-item">
                <a rel="nofollow" href="" title=""><i class="ion-social-instagram-outline"></i></a>
                &#xA0;
              </li>
              <li class="list-inline-item">
                <a rel="nofollow" href="" title="Facebook"><i class="ion-social-facebook-outline"></i></a>
                &#xA0;
              </li>
              <li class="list-inline-item">
                <a rel="nofollow" href="" title="Dribbble"><i class="ion-social-dribbble-outline"></i></a>
              </li>
            </ul>
          </div>
        </div>
        <br />
        <br />
        <hr class="light" />
        <div class="text-center text-normal">
          <a href="http://themes.guide">
            Made by Themes.guide in Little Rhody
            <i class="ion-location"></i>
          </a>
        </div>
      </div>
    </footer>
  </div>
</template>

<style></style>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      return localStorage.jwt;
    },
    getUserId: function() {
      return localStorage.user_id;
    },
  },
};
</script>
