<template>
  <div class="posts-show">
    <img :src="post.image_url" alt="" />
    <h1>{{ post.title }}</h1>

    <div v-if="$parent.getUserId() == post.user.id">
      <router-link :to="`/posts/${post.id}/edit`">
        <button>Edit</button>
      </router-link>
    </div>
    <p>{{ post.subtitle }}</p>
    By:
    <router-link :to="`/users/${post.user.id}`">
      <p>{{ post.user.name }}</p>
    </router-link>
    <p>{{ relativeDate(post.created_at) }}</p>
    <p>{{ post.body }}</p>
    Comments:
    <div v-for="comment in post.comments" v-bind:key="comment.id">
      <router-link :to="`/users/${comment.user.id}`">
        {{ comment.user.image_url }}
        <p>{{ comment.user.name }}</p>
      </router-link>
      <p>{{ comment.body }}</p>
      <p>{{ relativeTime(comment.created_at) }}</p>
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
    };
  },
  created: function() {
    axios.get(`/api/posts/${this.$route.params.id}`).then(response => {
      this.post = response.data;
      console.log(this.post);
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).format("MMMM Do, YYYY");
    },
    relativeTime: function(date) {
      return moment(date).fromNow();
    },
  },
};
</script>
