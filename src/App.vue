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
          </ul>
        </ul>
        <ul class="nav navbar-nav ml-auto">
          <li class="nav-item dropdown dropdown-hover megamenu">
            <a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">
              welcome
              <span class="caret"></span>
            </a>
            <div class="dropdown-menu dropdown-menu-right pb-2">
              <div class="row mx-1">
                <div class="col-12">
                  <span class="close d-sm-none" data-toggle="dropdown">&#xD7;</span>
                </div>
                <ul class="col-md-6 col-6 list-unstyled small" role="menu"></ul>
                <ul v-if="isLoggedIn()" class="col-md-6 col-6 list-unstyled small text-right" role="menu">
                  <!-- Signup</router-link> -->
                  <li class="text-uppercase text-muted text-wide pt-3 pb-1">Profile</li>
                  <li><a class="link" :href="`/users/${getUserId()}`">View/Edit</a></li>

                  <li class="text-uppercase text-muted text-wide pt-3 pb-1">Create</li>
                  <li><a class="link" href="/posts/new">New Post</a></li>

                  <li class="text-uppercase text-muted text-wide pt-3 pb-1">User</li>
                  <li><a class="link" href="/logout">Logout</a></li>
                </ul>
                <ul v-else class="col-md-6 col-6 list-unstyled small text-right" role="menu">
                  <!-- Signup</router-link> -->
                  <li><a class="link" v-if="!isLoggedIn()" href="/login">Sign-up Login</a></li>
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
          <div class="col-lg-10">
            <p class="display-3 mb-2">We know you want to help save the environment.</p>
            <hr />
          </div>
        </div>
        <div class="row">
          <div class="col-6 col-md-3">
            <h6 class="text-uppercase">User</h6>
            <ul class="list-unstyled">
              <li><a :href="`/users/${getUserId()}`">Profile</a></li>
            </ul>
          </div>
          <div class="col-6 col-md-3 column">
            <h6 class="text-uppercase">About</h6>
            <ul class="list-unstyled">
              <li><a href="mailto:gonatural@gmail.com">Contact Us</a></li>
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
