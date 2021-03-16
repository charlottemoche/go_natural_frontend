<template>
  <div class="users-show">
    <img :src="user.image_url" alt="" class="avatar" />
    <h2>{{ user.name }}</h2>
    <p>{{ user.bio }}</p>
    <div v-if="$parent.getUserId() == user.id">
      <router-link :to="`/users/${user.id}/edit`">
        <button>Edit Profile</button>
      </router-link>
    </div>
    <br />
    <div>
      <div v-for="post in user.posts" v-bind:key="post.id">
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
