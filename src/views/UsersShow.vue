<template>
  <div class="users-show">
    <div class="container outer my-2">
      <div class="row">
        <div class="col-lg-12">
          <hr />
        </div>
        <div class="col-lg-9 order-lg-2">
          <ul class="nav nav-tabs small text-uppercase">
            <li class="nav-item">
              <a href="" data-target="#profile" data-toggle="tab" class="nav-link active">Profile</a>
            </li>
            <!-- <div v-if="$parent.getUserId() == user.id"> -->
            <li class="nav-item" v-if="$parent.getUserId() == user.id">
              <a href="" data-target="#edit" data-toggle="tab" class="nav-link">Edit</a>
            </li>
            <!-- </div> -->
          </ul>
          <div class="tab-content py-3">
            <div class="tab-pane py-2 active" id="profile">
              <h4 class="mt-3">Recent Activity</h4>
              <div class="jumbotron">
                <div
                  class="card card-default border-light mb-3 pb-3 wow fadeIn"
                  v-for="post in user.posts"
                  v-bind:key="post.id"
                >
                  <div class="row no-gutters">
                    <div class="col-auto">
                      <span>
                        <router-link :to="`/posts/${post.id}`">
                          <img v-bind:src="post.image_url" class="img-fluid card-image" alt="" />
                        </router-link>
                      </span>
                    </div>
                    <div class="col">
                      <div class="card-block px-2">
                        <h6 class="card-title">{{ post.title }}</h6>
                        <p class="card-text">{{ post.subtitle }}</p>
                        <div v-if="$parent.getUserId() == $route.params.id">
                          <router-link :to="`/posts/${post.id}/edit`">
                            <button class="btn btn-sm btn-primary">
                              <i class="ion-edit"></i>
                            </button>
                          </router-link>
                        </div>
                        <br />
                        <small class="text-muted">Post Created {{ relativeDate(post.created_at) }}</small>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <!--/row-->
            </div>
            <div class="tab-pane py-2" id="edit">
              <h4 class="mt-3">User Information</h4>
              <div class="jumbotron">
                <form v-on:submit.prevent="updateUser()" role="form">
                  <div class="form-group row">
                    <label class="col-lg-3 col-form-label form-control-label">Profile Picture:</label>
                    <div class="col-lg-9">
                      <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput" />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label class="col-lg-3 col-form-label form-control-label">Name:</label>
                    <div class="col-lg-9">
                      <input type="text" class="form-control" v-model="user.name" />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label class="col-lg-3 col-form-label form-control-label">Bio:</label>
                    <div class="col-lg-9">
                      <input type="text" class="form-control" v-model="user.bio" />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label class="col-lg-3 col-form-label form-control-label"></label>
                    <div class="col-lg-9">
                      <input type="submit" class="btn btn-primary" value="Update Profile" />
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
        <div class="col-lg-3 order-lg-1 text-left">
          <img :src="user.image_url" alt="" class="avatar" />
          <br />
          <h2 class="my-1">{{ user.name }}</h2>
          <!-- <h6 class="text-left">{{ user.name }}</h6> -->

          <table class="table table-sm small text-left">
            <tbody>
              <tr>
                <td class="text-uppercase"><b>Bio</b></td>
                <td>{{ user.bio }}</td>
              </tr>
              <tr>
                <td class="text-uppercase"><b>Posts</b></td>
                <td>{{ user.posts.length }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
img {
  width: 200px;
}
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
}
.jumbotron {
  padding-top: 40px;
  padding-left: 40px;
  background: #ffffff;
}
.card-horizontal {
  display: flex;
  flex: 1 1 auto;
  width: 800px;
}
</style>
<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function() {
    return {
      user: {},
      posts: [],
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      this.user = response.data;
      console.log(this.user);
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).format("MMM D");
    },
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.imageFile = event.target.files[0];
      }
    },
    updateUser: function() {
      var formData = new FormData();
      formData.append("name", this.user.name);
      formData.append("bio", this.user.bio);
      if (this.imageFile) {
        formData.append("image_file", this.imageFile);
      }
      axios
        .patch(`/api/users/${this.user.id}`, formData)
        .then(response => {
          console.log(response.data);
          window.location.reload(`/users/${this.user.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
