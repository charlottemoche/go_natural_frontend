<template>
  <div class="users-show">
    <div class="container outer my-2">
      <div class="row">
        <div class="col-lg-12">
          <h2 class="my-1">{{ user.name }}</h2>
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
              <div class="row my-2">
                <div class="col-md-6"></div>
                <div class="col-md-12">
                  <h4 class="mt-3">Recent Activity</h4>
                  <table class="table table-hover table-striped">
                    <div>
                      <div v-for="post in user.posts" v-bind:key="post.id">
                        <router-link :to="`/posts/${post.id}`">
                          <span>
                            <img v-bind:src="post.image_url" alt="" />
                          </span>
                        </router-link>
                        <h6>{{ post.title }}</h6>
                        <small>{{ post.subtitle }}</small>
                        <br />
                        <small>{{ relativeDate(post.created_at) }}</small>

                        <div v-if="$parent.getUserId() == $route.params.id">
                          <router-link :to="`/posts/${post.id}/edit`">
                            <button>
                              <i class="ion-edit"></i>
                            </button>
                          </router-link>
                        </div>
                      </div>
                    </div>
                  </table>
                </div>
              </div>
              <!--/row-->
            </div>
            <div class="tab-pane py-2" id="messages">
              <div class="alert alert-info alert-dismissable">
                <a class="panel-close close" data-dismiss="alert">&#xD7;</a>
                This is an
                <span class="font-weight-bold">.alert</span>
                . Use this to show important messages to the user.
              </div>
              <table class="table table-hover table-striped">
                <tbody>
                  <tr>
                    <td>
                      <span class="float-right font-weight-bold">3 hrs ago</span>
                      Here is your a link to the latest summary report from the..
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <span class="float-right font-weight-bold">Yesterday</span>
                      There has been a request on your account since that was..
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <span class="float-right font-weight-bold">9/10</span>
                      Porttitor vitae ultrices quis, dapibus id dolor. Morbi venenatis lacinia rhoncus.
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <span class="float-right font-weight-bold">9/4</span>
                      Vestibulum tincidunt ullamcorper eros eget luctus.
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="tab-pane py-2" id="edit">
              <form role="form">
                <div class="form-group row">
                  <label class="col-lg-3 col-form-label form-control-label">First name</label>
                  <div class="col-lg-9">
                    <input class="form-control" type="text" value="John" />
                  </div>
                </div>
                <div class="form-group row">
                  <label class="col-lg-3 col-form-label form-control-label">Last name</label>
                  <div class="col-lg-9">
                    <input class="form-control" type="text" value="Bishop" />
                  </div>
                </div>
                <div class="form-group row">
                  <label class="col-lg-3 col-form-label form-control-label">Email</label>
                  <div class="col-lg-9">
                    <input class="form-control" type="email" value="email@gmail.com" />
                  </div>
                </div>
                <div class="form-group row">
                  <label class="col-lg-3 col-form-label form-control-label"></label>
                  <div class="col-lg-6">
                    <input class="form-control" type="text" value="" placeholder="City" />
                  </div>
                  <div class="col-lg-3">
                    <input class="form-control" type="text" value="" placeholder="State" />
                  </div>
                </div>
                <div class="form-group row">
                  <label class="col-lg-3 col-form-label form-control-label">Username</label>
                  <div class="col-lg-9">
                    <input class="form-control" type="text" value="jbishop32" />
                  </div>
                </div>
                <div class="form-group row">
                  <label class="col-lg-3 col-form-label form-control-label"></label>
                  <div class="col-lg-9">
                    <input type="reset" class="btn btn-secondary" value="Cancel" />
                    <input type="button" class="btn btn-primary" value="Save Changes" />
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="col-lg-3 order-lg-1 text-center">
          <img :src="user.image_url" alt="" class="avatar" />
          <hr />

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
  width: 150px;
}
.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
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
