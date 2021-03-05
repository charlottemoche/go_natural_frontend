<template>
  <div class="topics-show">
    <h1>{{ topic.title }}</h1>
    <div v-for="post in topic.posts" v-bind:key="post.id">
      <router-link :to="`/posts/${post.id}`">
        <span>
          <img v-bind:src="post.image_url" alt="" />
        </span>
      </router-link>
      <h2>{{ post.title }}</h2>
      <p>{{ post.subtitle }}</p>
      By:
      <p>{{ post.user.name }}</p>
      <p>{{ relativeDate(post.created_at) }}</p>
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
      topic: {},
      post: {
        user: {
          name: "",
        },
      },
    };
  },
  created: function() {
    axios.get(`/api/topics/${this.$route.params.id}`).then(response => {
      this.topic = response.data;
      console.log(this.topic);
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).format("MMM D");
    },
  },
};
</script>
