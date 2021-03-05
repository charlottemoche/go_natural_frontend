<template>
  <div class="users-show">
    {{ user.image_url }}
    Name: {{ user.name }}
    <br />
    Bio: {{ user.bio }}
    <div>
      <div v-for="post in user.posts" v-bind:key="post.id">
        <img :src="post.image_url" alt="" />
        <h2>{{ post.title }}</h2>
        <div v-if="$parent.getUserId()">
          <router-link :to="`/posts/${post.id}/edit`">
            <button>Edit</button>
          </router-link>
        </div>
        <p>{{ post.subtitle }}</p>
        <p>{{ post.body }}</p>
        <p>{{ relativeDate(post.created_at) }}</p>
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
