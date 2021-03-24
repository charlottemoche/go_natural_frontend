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
      <a class="navbar-brand page-scroll text-uppercase wide-space link" href="/">
        <img src="/assets/images/gonatural1.png" />
      </a>
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
          <ul class="nav navbar-nav ml-auto">
            <li class="nav-item">
              <a class="nav-link page-scroll" href="/">Blog</a>
            </li>
            <li class="nav-item dropdown dropdown-hover megamenu">
              <a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">
                User
                <span class="caret"></span>
              </a>
              <div class="dropdown-menu dropdown-menu-right pt-1 pb-2">
                <div class="row mx-1">
                  <div class="col-12">
                    <span class="close d-sm-none" data-toggle="dropdown">&#xD7;</span>
                  </div>
                  <ul class="col-md-6 col-6 list-unstyled small" role="menu"></ul>
                  <ul class="col-md-6 col-6 list-unstyled small text-right" role="menu">
                    <!-- Signup</router-link> -->
                    <div v-if="isLoggedIn()">
                      <li class="text-uppercase text-muted text-wide pt-3 pb-1">Profile</li>
                      <li><a class="link" :href="`/users/${getUserId()}`">View/Edit</a></li>
                    </div>
                    <div v-if="isLoggedIn()">
                      <li class="text-uppercase text-muted text-wide pt-3 pb-1">Create</li>
                      <li><a class="link" href="/posts/new">New Post</a></li>
                    </div>

                    <li class="text-uppercase text-muted text-wide pt-3 pb-1">User</li>
                    <li><a class="link" v-if="!isLoggedIn()" href="/login">Sign-up | Login</a></li>
                    <li><a class="link" v-if="isLoggedIn()" href="/logout">Logout</a></li>
                  </ul>
                </div>
              </div>
            </li>
          </ul>
        </ul>
      </div>
    </nav>

    <router-view />

    <footer id="footer" class="pt-5">
      <div class="container">
        <div class="row">
          <div class="col-lg-10">
            <p class="display-2 mb-2">We know you want to save the environment.</p>
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
        </div>
        <br />
        <br />
        <hr class="light" />
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
