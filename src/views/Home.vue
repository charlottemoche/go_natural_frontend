<template>
  <div class="home">
    <div class="my-4">
      <!--slider-->

      <br />

      <!--cards carousel slider-->
      <div class="carousel slide w-100 mb-5" data-ride="carousel" data-interval="10000" id="postsCarousel">
        <div class="container mt-2 px-0">
          <input type="text" v-model="filter" list="titles" placeholder="Browse Topics" />
          <datalist id="titles">
            <option v-for="topic in topics" v-bind:key="topic.title">{{ topic.title }}</option>
          </datalist>
          <hr />
          <div class="carousel-inner pb-2">
            <!-- three (3) sildes per carousel item -->
            <div class="carousel-item active flex-column flex-sm-row align-items-stretch">
              <div
                class="col-md-4"
                v-for="topic in orderBy(filterBy(topics, filter), sortAttribute)"
                v-bind:key="topic.id"
              >
                <div class="card card-default h-100">
                  <div class="card-img-top-200 card-zoom">
                    <router-link :to="`topics/${topic.id}`">
                      <span>
                        <img v-bind:src="topic.image_url" alt="" />
                      </span>
                    </router-link>
                  </div>
                  <div class="card-body pt-2 pl-1">
                    <h6 class="small text-wide text-truncate pb-1">Topic</h6>
                    <h2>
                      <a href="./blog-detail.html">{{ topic.title }}</a>
                    </h2>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!--/multi cards carousel slider-->
      </div>
    </div>
    <!-- <input type="text" v-model="filter" list="titles" />
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
    </div> -->
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
