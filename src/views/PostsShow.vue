<template>
  <div class="posts-show">
    <div class="container outer pb-3 mt-3" id="blog-post">
      <div class="row">
        <div class="col-xl-4 col-lg-4 col-md-5 py-2">
          <h2 class="display-1 hidden-down pt-2 post-title">
            {{ post.title }}
          </h2>
          <p class="post-subtitle"></p>
        </div>
        <div class="col-xl-4 ml-xl-auto col-lg-4 ml-lg-auto col-md-5 ml-md-auto py-2">
          <blockquote class="blockquote">
            <h3 class="text-right pt-2"><a href="/">Topics</a></h3>
          </blockquote>
        </div>
        <div class="col-sm-12">
          <div class="card card-default py-2">
            <p class="lead m-0">{{ post.subtitle }}</p>
          </div>
          <h6 class="text-wide pt-2">
            <br />
            <a class="float-right post-category">{{ post.likes.length }} likes</a>
            <br />
            <div v-if="$parent.isLoggedIn()">
              <div class="float-right post-category">
                <button v-bind:class="{ 'red-heart': liked }" class="btn btn" v-on:click="isLiked()">
                  &hearts;
                </button>
              </div>
            </div>
            <span class="text-dark post-author">
              <router-link :to="`/users/${post.user.id}`">
                <p>{{ post.user.name }}</p>
              </router-link>
            </span>
          </h6>

          <small>{{ relativeDate(post.created_at) }}</small>
        </div>
      </div>
      <div class="row pt-2">
        <div class="col-md-12">
          <img :src="post.image_url" class="post-img img-fluid" alt="post photo" />
        </div>
        <div class="col-md-12"></div>
        <div class="col-md-12 pt-3 post-content">
          <span v-html="post.body"></span>

          <hr />

          <hr />
        </div>
        <div class="col-md-5">
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
        <div class="col-md-7"></div>
      </div>
      <div class="row pt-2">
        <div class="col-md-6 pt-2 pb-2">
          <!-- social media sharing links -->
        </div>
        <div class="col-md-6 pt-2 pb-2"></div>
        <div class="col-lg-8 col-md-9 pt-2">
          <div class="comments row" id="comments">
            <div class="col-12 mb-4">
              <h3 class="mb-4">Comments</h3>
              <div class="row pt-2">
                <div class="col-12">
                  <div v-if="$parent.isLoggedIn()">
                    <form v-on:submit.prevent="createComment()">
                      <label for="body"></label>
                      <input type="text" id="body" name="body" value="" v-model="body" />
                      <label for="post-id"></label>
                      <input type="hidden" id="post-id" name="post-id" value="" v-model="postId" />
                      <input type="submit" value="Post" />
                    </form>
                  </div>
                </div>
              </div>
              <hr />
              <!-- comment -->
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
                          <input type="submit" class="btn btn-sm btn-secondary" value="Update" />
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
              <!-- /comment -->
            </div>
          </div>
        </div>
        <div class="col-lg-4 col-md-3 pt-2">
          <div class="row">
            <!--related post-->
            <!--related post-->
          </div>
        </div>
      </div>
    </div>
    <!-- 
    <img :src="post.image_url" alt="" />
    <h2>{{ post.title }}</h2>
    <div v-if="$parent.getUserId() == post.user.id">
      <router-link :to="`/posts/${post.id}/edit`">
        <button>Edit</button>
      </router-link>
    </div>
    <p>{{ post.subtitle }}</p>

    <small>
      by:
      <router-link :to="`/users/${post.user.id}`">
        <p>{{ post.user.name }}</p>
        <img :src="post.user.image_url" alt="" class="avatar" />
      </router-link>
    </small>
    <br />
    <small>{{ relativeDate(post.created_at) }}</small>
    <br />

    <p>{{ post.likes.length }} likes</p>
    <div v-if="$parent.isLoggedIn()">
      <button v-bind:class="{ 'red-heart': liked }" v-on:click="isLiked()">&hearts;</button>
    </div>
    <br />

    <span v-html="post.body"></span>
    <ul>
      <li class="text-danger" v-for="error in errors" v-bind:key="error">
        <p>{{ error }}</p>
      </li>
    </ul>

    <div v-if="$parent.isLoggedIn()">
      <form v-on:submit.prevent="createComment()">
        <label for="body"></label>
        <input type="text" id="body" name="body" value="" v-model="body" />
        <label for="post-id"></label>
        <input type="hidden" id="post-id" name="post-id" value="" v-model="postId" />
        <input type="submit" value="Post" />
      </form>
    </div>
    <br />
    Comments:
    <div v-for="comment in post.comments" v-bind:key="comment.id">
      <router-link :to="`/users/${comment.user.id}`">
        <img :src="comment.user.image_url" alt="" class="avatar" />
        <small>{{ comment.user.name }}</small>
      </router-link>

      <div v-if="commentEditToggle == comment.id">
        <form v-on:submit.prevent="updateComment(comment)">
          <div class="form-group">
            <input type="text" class="form-control" v-model="comment.body" />
            <input type="submit" class="btn btn-primary" value="Update" />
          </div>
        </form>
      </div>

      <div v-else>
        <p>{{ comment.body }}</p>
      </div>

      <div v-if="$parent.getUserId() == comment.user.id">
        <button v-on:click="destroyComment(comment)">Delete</button>
        <button v-on:click="commentEditToggle = comment.id">Edit</button>
      </div> -->
    <!-- </div> -->
  </div>
</template>

<style scoped>
/* img {
  width: 350px;
} */
.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.profile {
  width: 200px;
}
.red-heart {
  color: red;
}
</style>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function() {
    return {
      post: {
        likes: [],
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
      if (confirm("Are you sure?")) {
        axios.delete(`/api/comments/${comment.id}`).then(response => {
          this.post.comments.splice(index, 1);
          console.log(response.data);
        });
      }
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
