<template>
  <div class="home">
    <h1>Topics:</h1>
    <input type="text" v-model="filter" list="titles" />
    <datalist id="titles">
      <option v-for="topic in topics" v-bind:key="topic.title">{{ topic.title }}</option>
    </datalist>
    <div v-for="topic in orderBy(filterBy(topics, filter), sortAttribute)" v-bind:key="topic.id">
      <h3>{{ topic.title }}</h3>
      <router-link :to="`topics/${topic.id}`">
        <span>
          <img v-bind:src="topic.image_url" alt="" />
        </span>
      </router-link>
    </div>
    <h1>Trending:</h1>
    <div v-for="post in posts" v-bind:key="post.id">
      <h3>{{ post.title }}</h3>
      <p>{{ post.subtitle }}</p>
      <router-link :to="`posts/${post.id}`">
        <span>
          <img v-bind:src="post.image_url" alt="" class="posts" />
        </span>
      </router-link>
    </div>
  </div>
</template>

<style scoped>
img {
  width: 350px;
}
.posts {
  width: 150px;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      topics: [],
      posts: [],
      filter: "",
      sortAttribute: "",
    };
  },
  created: function() {
    this.indexTopics();
  },

  methods: {
    indexTopics: function() {
      axios.get("/api/topics").then(response => {
        console.log(response.data);
        this.topics = response.data.topics;
        this.posts = response.data.posts;
      });
    },
  },
};
</script>
