<template>
  <div class="posts-show">
    <div class="container pb-3">
      <div class="row">
        <div class="col-xl-4 col-lg-4 col-md-5 py-2">
          <hr />
          <h1>
            {{ post.title }}
          </h1>
          <p class="post-subtitle"></p>
        </div>
        <div class="col-xl-4 ml-xl-auto col-lg-4 ml-lg-auto col-md-5 ml-md-auto py-2">
          <blockquote class="blockquote">
            <h3 class="text-right pt-2">
              <router-link :to="`/topics/${post.topic.id}`">{{ post.topic.title }}</router-link>
            </h3>
          </blockquote>
        </div>
        <div class="col-sm-12">
          <div class="card card-default py-2">
            <p class="font-italic lead m-0">{{ post.subtitle }}</p>
          </div>
          <br />
          <div v-if="$parent.getUserId() == post.user.id">
            <router-link :to="`/posts/${post.id}/edit`">
              <button class="btn btn-sm btn-secondary">
                <i class="ion-edit"></i>
              </button>
            </router-link>
          </div>
          <br />
        </div>
      </div>
      <div class="jumbotron jumbotron-fluid">
        <div class="col-sm-12">
          <h6 class="text-wide pt-2">
            <img :src="post.user.image_url" class="avatar float-left" />
            <span>
              <router-link :to="`/users/${post.user.id}`">
                <a class="float-left">{{ post.user.name }}</a>
              </router-link>
            </span>
            <a class="float-right post-category">{{ post.likes.length }} likes</a>
            <br />
            <small class="float-left">{{ relativeDate(post.created_at) }}</small>
            <div v-if="$parent.isLoggedIn()">
              <div class="float-right">
                <button v-bind:class="{ 'red-heart': liked }" class="btn btn" v-on:click="isLiked()">
                  &hearts;
                </button>
              </div>
            </div>
          </h6>

          <br />

          <br />
          <div class="container-image">
            <img :src="post.image_url" class="post-image img-fluid" alt="post photo" />
          </div>

          <hr />
          <div class="container-body">
            <span class="text" v-html="post.body"></span>
          </div>
        </div>
        <br />

        <br />

        <div class="col-md-5">
          <h3 class="text-left">About the Author:</h3>
          <!--spacer-->
          <div class="bg-alt p-3">
            <div class="row">
              <div class="col-md-3">
                <span>
                  <router-link :to="`/users/${post.user.id}`">
                    <img :src="post.user.image_url" class="profile" alt="" />
                  </router-link>
                </span>
              </div>
            </div>
            <h6 class="text-wide text-uppercase pt-2">
              <span class="text-dark">{{ post.user.name }}</span>
            </h6>
            <p class="text-secondary">
              {{ post.user.bio }}
            </p>
          </div>
        </div>
      </div>
      <h3 class="mb-4">Comments:</h3>
      <div v-if="$parent.isLoggedIn()">
        <form v-on:submit.prevent="createComment()">
          <label for="body"></label>
          <input class="comment-input" type="text" id="body" name="body" value="" v-model="body" />
          <label for="post-id"></label>
          <input type="hidden" id="post-id" name="post-id" value="" v-model="postId" />
          <input type="submit" value="Post" />
        </form>
      </div>
      <!-- comments -->
      <div class="comments row" id="comments">
        <div class="col-12 mb-4">
          <div class="row pt-2">
            <div class="col-12"></div>
          </div>
          <hr />
          <!-- comment -->
          <div class="jumbotron jumbotron-fluid">
            <div class="container-comments">
              <div class="comment mb-3 row" v-for="comment in post.comments" v-bind:key="comment.id">
                <div class="comment-avatar col-lg-1 ml-lg-auto col-md-2 ml-md-0 col-4 ml-auto text-center">
                  <a :href="`/users/${comment.user.id}`">
                    <img class="mx-auto rounded-circle img-fluid" :src="comment.user.image_url" alt="avatar" />
                  </a>
                </div>
                <div class="comment-content col-lg-11 col-md-10 col-12">
                  <h6 class="small comment-meta">
                    <a :href="`/users/${comment.user.id}`">{{ comment.user.name }}</a>
                    |
                    {{ relativeTime(comment.created_at) }}
                  </h6>

                  <div class="comment-body">
                    <div v-if="commentEditToggle == comment.id">
                      <form v-on:submit.prevent="updateComment(comment)">
                        <div class="form-group">
                          <input type="text" class="form-control" v-model="comment.body" />
                          <br />
                          <input type="submit" class="btn btn-sm btn-secondary" value="Update" />
                          &nbsp;
                          <button v-on:click="destroyComment(comment)" class="btn btn-sm btn-danger">Delete</button>
                        </div>
                      </form>
                    </div>
                    <div v-else>
                      <div v-if="$parent.getUserId() == comment.user.id">
                        <button class="btn btn-sm btn-light float-right" v-on:click="commentEditToggle = comment.id">
                          <i class="ion-edit"></i>
                        </button>
                      </div>
                      <p>{{ comment.body }}</p>
                    </div>
                  </div>
                </div>
              </div>
              <!-- /comments -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.post-image {
  width: 600px;
}
.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 10px;
}
.profile {
  width: 200px;
}
.red-heart {
  color: red;
}
.jumbotron-fluid {
  background: #ffffff;
  padding-top: 20px;
  padding-left: 50px;
  padding-right: 50px;
}
.container {
  padding-top: 20px;
  padding-left: 100px;
  padding-right: 100px;
}
.container-image {
  text-align: center;
}
.container-comments {
  padding-top: 20px;
  padding-bottom: 0px;
}
.container-body {
  padding-left: 50px;
  padding-right: 50px;
}
.profile {
  width: 150px;
}
.comment-input {
  width: 400px;
}
</style>

<script>
import axios from "axios";
import moment from "moment";
import swal from "sweetalert";

export default {
  data: function() {
    return {
      post: {
        likes: [],
        topic: {
          title: "",
        },
        user: {
          name: "",
          bio: "",
        },
      },
      comments: {
        user: {
          name: "",
        },
      },
      body: "",
      postId: `${this.$route.params.id}`,
      errors: [],
      commentEditToggle: null,
      liked: false,
    };
  },
  created: function() {
    axios.get(`/api/posts/${this.$route.params.id}`).then(response => {
      this.post = response.data;
      this.liked = this.post.likes.find(like => like.user_id == this.$parent.getUserId());
      console.log(this.liked);
      console.log(this.post);
    });
  },

  methods: {
    isLiked: function() {
      if (this.liked) {
        this.unlikePost();
        this.liked = false;
      } else {
        this.likePost();
        this.liked = true;
      }
    },
    likePost: function() {
      var params = {
        id: this.post.id,
      };
      axios.post(`/api/posts/${this.$route.params.id}/like`, params).then(response => {
        console.log(response.data);
        this.post.likes.unshift(response.data);
      });
    },
    unlikePost: function(like) {
      var index = this.post.likes.indexOf(like);
      var params = {
        id: this.post.id,
      };
      axios.delete(`/api/posts/${this.$route.params.id}/like`, params).then(response => {
        this.post.likes.splice(index, 1);
        console.log(response.data);
      });
    },
    createComment: function() {
      var params = {
        body: this.body,
        post_id: this.postId,
      };
      axios
        .post("/api/comments", params)
        .then(response => {
          this.post.comments.unshift(response.data);
          console.log(response.data);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    updateComment: function(comment) {
      var index = this.post.comments.indexOf(comment);
      var params = {
        body: comment.body,
      };
      axios.patch(`/api/comments/${comment.id}`, params).then(response => {
        this.post.comments[index] = response.data;
        this.commentEditToggle = null;
        console.log(response.data);
      });
    },
    destroyComment: function(comment) {
      var index = this.post.comments.indexOf(comment);
      swal({
        title: "Are you sure?",
        icon: "warning",
        buttons: true,
        dangerMode: true,
      }).then(willDelete => {
        if (willDelete) {
          swal("Your comment has been deleted", {
            icon: "success",
          });
          axios.delete(`/api/comments/${comment.id}`).then(response => {
            this.post.comments.splice(index, 1);
            console.log(response.data);
          });
        } else {
          swal("Your post has been saved");
        }
      });
      // var index = this.post.comments.indexOf(comment);
      // if (confirm("Are you sure?")) {
      //   axios.delete(`/api/comments/${comment.id}`).then(response => {
      //     this.post.comments.splice(index, 1);
      //     console.log(response.data);
      //   });
      // }
    },
    relativeDate: function(date) {
      return moment(date).format("MMMM Do, YYYY");
    },
    relativeTime: function(date) {
      return moment(date).fromNow();
    },
  },
};
</script>
