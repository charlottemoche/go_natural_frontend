<template>
  <div class="login">
    <section class="bg-alt">
      <div class="container">
        <h2 class="text-center pb-2"></h2>
        <div class="row wow slideInUp">
          <div class="col-lg-6 col-12 pb-3">
            <div class="card h-100">
              <div class="card-img-top"></div>
              <div class="card-body">
                <h1>Login</h1>
                <form role="form" v-on:submit.prevent="loginSubmit()">
                  <div class="form-group">
                    <label for="inputEmailForm" class="sr-only form-control-label">Email</label>
                    <div class="mx-auto col-sm-10">
                      <input
                        type="text"
                        class="form-control"
                        id="inputEmailForm"
                        placeholder="email"
                        required
                        v-model="loginEmail"
                      />
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="inputPasswordForm" class="sr-only form-control-label">Password</label>
                    <div class="mx-auto col-sm-10">
                      <input
                        type="password"
                        class="form-control"
                        id="inputPasswordForm"
                        placeholder="password"
                        required
                        v-model="loginPassword"
                      />
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="mx-auto col-sm-10">
                      <div class="checkbox form-control form-control-sm text-center small">
                        <label class="">
                          <input type="checkbox" class="" />
                          remember me
                        </label>
                      </div>
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="mx-auto col-sm-10 pb-3 pt-2">
                      <button type="submit" class="btn btn-outline-secondary btn-lg btn-block" value="Submit">
                        Sign-in
                      </button>
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
          <div class="col-lg-6 col-12 pb-3">
            <div class="card h-100">
              <div class="card-img-top"></div>
              <div class="card-body">
                <h1>Sign-up</h1>
                <form role="form" v-on:submit.prevent="signupSubmit()">
                  <div class="form-group">
                    <label for="name" class="sr-only form-control-label">name</label>
                    <div class="mx-auto col-sm-10">
                      <input type="text" class="form-control" value="" placeholder="name" required v-model="name" />
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="input2EmailForm" class="sr-only form-control-label">email</label>
                    <div class="mx-auto col-sm-10">
                      <input
                        type="text"
                        class="form-control"
                        value=""
                        id="input2EmailForm"
                        placeholder="email"
                        required
                        v-model="signupEmail"
                      />
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="input2PasswordForm" class="sr-only form-control-label">
                      password
                    </label>
                    <div class="mx-auto col-sm-10">
                      <input
                        type="password"
                        class="form-control"
                        value=""
                        id="input2PasswordForm"
                        placeholder="password"
                        required
                        v-model="signupPassword"
                      />
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="input2Password2Form" class="sr-only form-control-label">verify</label>
                    <div class="mx-auto col-sm-10">
                      <input
                        type="password"
                        class="form-control"
                        value=""
                        id="input2Password2Form"
                        placeholder="confirm password"
                        required
                        v-model="passwordConfirmation"
                      />
                    </div>
                  </div>
                  <div class="form-group">
                    <div class="mx-auto col-sm-10 pb-3 pt-2">
                      <button type="submit" class="btn btn-outline-secondary btn-lg btn-block">
                        Register
                      </button>
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
        <hr />
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import swal from "sweetalert";

export default {
  data: function() {
    return {
      name: "",
      loginEmail: "",
      signupEmail: "",
      loginPassword: "",
      signupPassword: "",
      passwordConfirmation: "",
      errors: [],
    };
  },
  methods: {
    loginSubmit: function() {
      var params = {
        email: this.loginEmail,
        password: this.loginPassword,
      };
      axios
        .post("/api/sessions", params)
        .then(response => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          this.$router.push("/");
        })
        .catch(error => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
    signupSubmit: function() {
      var params = {
        name: this.name,
        email: this.signupEmail,
        password: this.signupPassword,
        password_confirmation: this.passwordConfirmation,
      };
      axios
        .post("/api/users", params)
        .then(response => {
          console.log(response.data);
          swal("You're all signed up! Please log in");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
