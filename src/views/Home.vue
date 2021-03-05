<template>
  <div class="home">
    Search:
    <input type="text" v-model="filter" list="titles" />
    <datalist id="titles">
      <option v-for="topic in topics" v-bind:key="topic.title">{{ topic.title }}</option>
    </datalist>
    <div v-for="topic in orderBy(filterBy(topics, filter), sortAttribute)" v-bind:key="topic.id">
      <h1>{{ topic.title }}</h1>
      <router-link :to="`topics/${topic.id}`">
        <span>
          <img v-bind:src="topic.image_url" alt="" />
        </span>
      </router-link>
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
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      topics: [],
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
        this.topics = response.data;
      });
    },
  },
};
</script>
