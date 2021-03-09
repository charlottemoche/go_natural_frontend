<template>
  <div class="users-show">
    {{ user.image_url }}
    Name: {{ user.name }}
    <br />
    Bio: {{ user.bio }}
    <div>
      <div v-for="post in user.posts" v-bind:key="post.id">
        <router-link :to="`/posts/${post.id}`">
          <span>
            <img v-bind:src="post.image_url" alt="" />
          </span>
        </router-link>
        <h2>{{ post.title }}</h2>

        <p>{{ relativeDate(post.created_at) }}</p>
        <div v-if="$parent.getUserId() == $route.params.id">
          <router-link :to="`/posts/${post.id}/edit`">
            <button>Edit</button>
          </router-link>
        </div>
        <p>{{ post.subtitle }}</p>
        <p>{{ post.body }}</p>
      </div>
    </div>
  </div>
</template>
<style scoped>
img {
  width: 150px;
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
