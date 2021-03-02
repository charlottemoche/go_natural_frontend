<template>
  <div class="home">
    <div v-for="topic in topics" v-bind:key="topic.id">
      <p>{{ topic.title }}</p>
      <img v-bind:src="topic.image_url" alt="" />
    </div>
  </div>
</template>

<style>
img {
  width: 350px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      topics: [],
    };
  },
  created: function() {
    this.indexTopics();
  },

  methods: {
    indexTopics: function() {
      axios.get("/api/topics").then(response => {
        console.log(response.data);
        this.topics = response.data;
      });
    },
  },
};
</script>
