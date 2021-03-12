<template>
  <div class="posts-show">
    <img :src="post.image_url" alt="" />
    <h2>{{ post.title }}</h2>

    <div v-if="$parent.getUserId() == post.user.id">
      <router-link :to="`/posts/${post.id}/edit`">
        <button>Edit</button>
      </router-link>
    </div>
    <p>{{ post.subtitle }}</p>
    By:
    <router-link :to="`/users/${post.user.id}`">
      <p>{{ post.user.name }}</p>
      {{ post.user.image_url }}
    </router-link>
    <br />
    <small>{{ relativeDate(post.created_at) }}</small>
    <p>{{ post.body }}</p>

    <h3>Add Comment</h3>
    <small class="red-text" v-if="!$parent.isLoggedIn()">Log in or create account to add a comment</small>
    <ul>
      <li class="text-danger" v-for="error in errors" v-bind:key="error">
        <p>{{ error }}</p>
      </li>
    </ul>
    <form v-on:submit.prevent="createComment()">
      <label for="body"></label>
      <input type="text" id="body" name="body" value="" v-model="body" />
      <label for="post-id"></label>
      <input type="hidden" id="post-id" name="post-id" value="" v-model="postId" />
      <input type="submit" value="Post" />
    </form>
    <br />
    Comments:
    <div v-for="comment in post.comments" v-bind:key="comment.id">
      <router-link :to="`/users/${comment.user.id}`">
        {{ comment.user.image_url }}
        <p>{{ comment.user.name }}</p>
      </router-link>
      <p>{{ comment.body }}</p>
      <small>{{ relativeTime(comment.created_at) }}</small>
    </div>
  </div>
</template>

<style scoped>
img {
  width: 350px;
}
</style>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function() {
    return {
      post: {
        user: {
          name: "",
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
    };
  },
  created: function() {
    axios.get(`/api/posts/${this.$route.params.id}`).then(response => {
      this.post = response.data;
      console.log(this.post);
    });
  },

  methods: {
    createComment: function() {
      var params = {
        body: this.body,
        post_id: this.postId,
      };
      axios
        .post("/api/comments", params)
        .then(response => {
          console.log(response.data);
          window.location.reload();
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
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
