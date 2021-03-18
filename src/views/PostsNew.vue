<template>
  <div class="posts-new">
    <h1>New Post</h1>
    <small class="red-text" v-if="!$parent.isLoggedIn()">You must log in to add a new post!</small>
    <ul>
      <li class="text-danger" v-for="error in errors" v-bind:key="error">
        {{ error }}
      </li>
    </ul>
    <form v-on:submit.prevent="createPost()">
      <label for="title">Title:</label>
      <input type="text" id="title" name="title" value="" v-model="title" />
      <br />
      <br />
      <label for="subtitle">Subtitle:</label>
      <input type="text" id="subtitle" name="subtitle" value="" v-model="subtitle" />
      <br />
      <br />
      <label for="body">Body:</label>
      <froala id="edit" :tag="'textarea'" :config="config" v-model="body"></froala>
      <br />
      <br />
      <label for="image-url">Image Url:</label>
      <input type="text" id="image-url" name="image-url" value="" v-model="imageUrl" />
      <br />
      <br />
      <label for="topic-id">Choose a topic:</label>
      <select name="topics" v-model="topicId">
        <option value="1">Replacement Items</option>
        <option value="2">Natural Cleaning Solutions</option>
        <option value="3">Food Waste Reduction</option>
      </select>
      <br />
      <br />
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
<style>
.red-text {
  color: red;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      title: "",
      subtitle: "",
      body: "",
      imageUrl: "",
      topicId: "",
      errors: [],
      config: {
        events: {
          initialized: function() {
            console.log("initialized");
          },
        },
      },
    };
  },
  methods: {
    createPost: function() {
      var params = {
        title: this.title,
        subtitle: this.subtitle,
        body: this.body,
        image_url: this.imageUrl,
        topic_id: this.topicId,
      };
      axios
        .post("/api/posts", params)
        .then(response => {
          console.log(response.data);
          this.$router.push(`/posts/${response.data.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
