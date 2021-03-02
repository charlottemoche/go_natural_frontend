<template>
  <div class="posts-show">
    <img :src="post.image_url" alt="" />
    <h1>{{ post.title }}</h1>
    <p>{{ post.subtitle }}</p>
    By:
    <p>{{ post.user.name }}</p>
    <p>{{ relativeDate(post.created_at) }}</p>
    <p>{{ post.body }}</p>
    Comments:
    <div v-for="comment in post.comments" v-bind:key="comment.id">
      <p>{{ comment.body }}</p>
      <p>{{ relativeTime(comment.created_at) }}</p>
      <p>{{ comment.user.name }}</p>
    </div>
  </div>
</template>

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
