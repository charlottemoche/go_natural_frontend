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
            <li class="nav-item" data-target="#editImage" data-toggle="collapse">
              <a href="" data-target="#edit" data-toggle="tab" class="nav-link">Edit</a>
            </li>
          </ul>
          <div class="tab-content py-3">
            <div class="tab-pane py-2 active" id="profile">
              <h4 class="mt-3">Recent Activity</h4>
              <div class="jumbotron">
                <div class="row my-2">
                  <div class="col-md-6"></div>
                  <div class="card" v-for="post in user.posts" v-bind:key="post.id">
                    <div class="card-horizontal">
                      <div class="card-zoom">
                        <router-link :to="`/posts/${post.id}`">
                          <span>
                            <img v-bind:src="post.image_url" alt="" />
                          </span>
                        </router-link>
                      </div>
                      <div class="card-body">
                        <h6 class="card-title">{{ post.title }}</h6>
                        <p class="card-text">
                          {{ post.subtitle }}
                        </p>
                        <div v-if="$parent.getUserId() == $route.params.id">
                          <router-link :to="`/posts/${post.id}/edit`">
                            <button class="btn btn-sm btn-primary">
                              <i class="ion-edit"></i>
                            </button>
                          </router-link>
                        </div>
                      </div>
                    </div>
                    <div class="card-footer">
                      <small class="text-muted">Post Created {{ relativeDate(post.created_at) }}</small>
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

    <!-- <img :src="user.image_url" alt="" class="avatar" />
    <h2>{{ user.name }}</h2>
    <p>{{ user.bio }}</p>
    <div v-if="$parent.getUserId() == user.id">
      <router-link :to="`/users/${user.id}/edit`">
        <button>Edit Profile</button>
      </router-link>
    </div>
    <br />
    <div> -->
    <!-- <div v-for="post in user.posts" v-bind:key="post.id">
        <router-link :to="`/posts/${post.id}`">
          <span>
            <img v-bind:src="post.image_url" alt="" />
          </span>
        </router-link>
        <h3>{{ post.title }}</h3>

        <small>{{ relativeDate(post.created_at) }}</small>
        <div v-if="$parent.getUserId() == $route.params.id">
          <router-link :to="`/posts/${post.id}/edit`">
            <button>Edit</button>
          </router-link>
        </div>
        <p>{{ post.subtitle }}</p> -->
    <!-- </div> -->
    <!-- </div> -->
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
  padding-top: 20px;
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
  },
};
</script>
