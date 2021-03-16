<template>
  <div class="posts-edit">
    <form v-on:submit.prevent="updatePost()">
      <h1>Edit post</h1>
      <img :src="post.image_url" alt="" />
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="post.title" />
      </div>
      <div class="form-group">
        <label>Subtitle:</label>
        <input type="text" class="form-control" v-model="post.subtitle" />
      </div>
      <div class="form-group">
        <label>Body:</label>
        <input type="text" class="form-control" v-model="post.body" />
      </div>
      <div class="form-group">
        <label>Image:</label>
        <input type="text" class="form-control" v-model="post.image_url" />
      </div>
      <label for="topic-id">Topic:</label>
      <select name="topics" v-model="post.topic_id">
        <option value="1">Item Replacements</option>
        <option value="2">Natural Cleaning Solutions</option>
        <option value="3">Food Waste Reduction</option>
      </select>
      <br />
      <input type="submit" class="btn btn-primary" value="Update" />
    </form>

    <button v-on:click="destroyPost()">Delete</button>
  </div>
</template>

<style scoped>
img {
  width: 350px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      post: {},
      errors: [],
      content: "",
      options: {},
    };
  },
  created: function() {
    axios.get(`/api/posts/${this.$route.params.id}`).then(response => {
      this.post = response.data;
      console.log(this.post);
    });
  },
  methods: {
    updatePost: function() {
      var params = {
        title: this.post.title,
        subtitle: this.post.subtitle,
        body: this.post.body,
        image_url: this.post.image_url,
        topic_id: this.post.topic_id,
      };
      axios
        .patch(`/api/posts/${this.post.id}`, params)
        .then(response => {
          console.log(response.data);
          this.$router.push(`/posts/${this.post.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyPost: function() {
      if (confirm("Are you sure?")) {
        axios.delete(`/api/posts/${this.post.id}`).then(response => {
          console.log(response.data);
          this.$router.push("/");
        });
      }
    },
  },
};
</script>
