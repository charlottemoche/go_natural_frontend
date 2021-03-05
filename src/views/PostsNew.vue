<template>
  <div class="posts-new">
    <h1>New Post</h1>
    <p class="red-text" v-if="!$parent.isLoggedIn()">You must log in to add a new post!</p>
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
      <input type="text" id="body" name="body" value="" v-model="body" />
      <br />
      <small v-bind:class="{ 'red-text': body.length > 100 }">{{ 100 - body.length }} characters left</small>
      <small v-if="body.length < 0" class="red-text"></small>
      <br />
      <br />
      <label for="image-url">Image Url:</label>
      <input type="text" id="image-url" name="image-url" value="" v-model="imageUrl" />
      <br />
      <br />
      <label for="topic-id">Choose a topic:</label>
      <select name="topics" v-model="topicId">
        <option value="1">Replacements</option>
        <option value="2">Natural Cleaning</option>
      </select>
      <br />
      <br />
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
<style scoped>
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
