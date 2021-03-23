<template>
  <div class="home">
    <div class="my-4">
      <!--slider-->
      <div class="container mt-2 px-0">
        <br />
        <br />
        <br />

        <!--cards carousel slider-->
        <div class="carousel slide w-100 mb-5" data-ride="carousel" data-interval="10000" id="postsCarousel">
          <div class="carousel-inner pb-2">
            <!-- three (3) sildes per carousel item -->
            <div class="carousel-item active flex-column flex-sm-row align-items-stretch">
              <div
                class="col-md-4"
                v-for="topic in orderBy(filterBy(topics, filter), sortAttribute)"
                v-bind:key="topic.id"
              >
                <div class="card card-default h-100">
                  <div class="card-img-top card-img-top-200 card-zoom">
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

    <section class="bg-alt">
      <div class="container pt-2">
        <div class="row wow fadeInUp pt-4">
          <div class="col-lg-7 col-md-6 col-12">
            <div class="card card-default h-100">
              <div class="card-img-top card-img-top-250 card-zoom">
                <a href="./blog-detail.html">
                  <img src="/assets/images/office4.jpg" class="mx-auto img-fluid" alt="image" />
                </a>
              </div>
              <div class="card-body py-3">
                <h6 class="text-uppercase small">Call to Action</h6>
                <h3 class="card-title text-truncate">Start Thinking in the Right Direction</h3>
                <a href="./blog-detail.html" class="btn btn-outline-secondary btn-lg mt-2 text-uppercase">Read</a>
              </div>
            </div>
          </div>
          <div class="col-lg-5 col-md-6 col-12">
            <div class="card card-default h-100">
              <div class="card-img-top card-img-top-250 card-zoom">
                <a href="./blog-detail.html">
                  <img src="/assets/images/mercantile_500_0001.png" class="mx-auto img-fluid" alt="image" />
                </a>
              </div>
              <div class="card-body py-3">
                <h6 class="text-uppercase small">Call to Action</h6>
                <h3 class="card-title text-truncate">Rhode Island Micro-Business Boom</h3>
                <a href="./blog-detail.html" class="btn btn-outline-secondary btn-lg mt-2 text-uppercase">Read</a>
              </div>
            </div>
          </div>
        </div>
        <div class="row wow fadeInUp pt-4">
          <div class="col-lg-5 col-md-6 col-12">
            <div class="card card-default h-100">
              <div class="card-img-top card-img-top-250 card-zoom">
                <a href="./blog-detail.html">
                  <img src="/assets/images/mercantile_500_0003.png" class="mx-auto img-fluid" alt="image" />
                </a>
              </div>
              <div class="card-body py-3">
                <h6 class="text-uppercase small">Call to Action</h6>
                <h3 class="card-title text-truncate">Not What You Expect</h3>
                <a href="./blog-detail.html" class="btn btn-outline-secondary btn-lg mt-2 text-uppercase">Read</a>
              </div>
            </div>
          </div>
          <div class="col-lg-7 col-md-6 col-12">
            <div class="card card-default h-100">
              <div class="card-img-top card-img-top-250 card-zoom">
                <a href="./blog-detail.html">
                  <img src="/assets/images/office1.jpg" class="mx-auto img-fluid" alt="image" />
                </a>
              </div>
              <div class="card-body py-3">
                <h6 class="text-uppercase small">Call to Action</h6>
                <h3 class="card-title text-truncate">Collaborative Workspaces Come of Age</h3>
                <a href="./blog-detail.html" class="btn btn-outline-secondary btn-lg mt-2 text-uppercase">Read</a>
              </div>
            </div>
          </div>
        </div>
        <hr />
        <br />
      </div>
    </section>

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

<style></style>

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
