<template>
  <div class="home">
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-7 py-4">
          <!-- main content -->

          <h1 class="display-3 mt-4 mb-2">Hi there, Environmentalist.</h1>

          <blockquote class="blockquote blockquote-reverse">
            <p class="mb-0">
              &quot;The truth is: the natural world is changing. And we are totally dependent on that world. It provides
              our food, water and air. It is the most precious thing we have and we need to defend it.&quot;
            </p>
            <div class="blockquote-footer">
              David Attenborough
            </div>
          </blockquote>

          <hr class="accent" />

          <p>
            We are Go Natural. We want you to help us in saving the environment. Together we can work to create more
            environmentally conscious behavior. Join us by checking out some of our posts and topics, and feel free to
            contribute your own!
          </p>

          <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis pharetra codeply varius quam sit amet
            vulputate. Quisque mauris augue, molestie tincidunt condimentum vitae, gravida a libero. Aenean sit amet
            felis dolor, in sagittis nisi. Sed ac orci quis tortor imperdiet venenatis. Duis elementum auctor accumsan.
            Aliquam in felis sit amet augue. Tincidunt codeply condimentum vitae, gravida a libero.
          </p>

          <hr />
          <h1>Check Out What's Trending:</h1>
          <br />

          <div class="card card-default border-light mb-3 pb-3 wow fadeIn" v-for="post in posts" v-bind:key="post.id">
            <div class="row no-gutters">
              <div class="col-auto">
                <span>
                  <router-link :to="`/posts/${post.id}`">
                    <img v-bind:src="post.image_url" class="img-fluid card-image" alt="" />
                  </router-link>
                </span>
              </div>
              <div class="col">
                <div class="card-block px-2">
                  <h6 class="card-title">{{ post.title }}</h6>
                  <p class="card-text">{{ post.subtitle }}</p>
                  <small><router-link :to="`/posts/${post.id}`" class="link">Read More</router-link></small>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="col-lg-4 col-md-5 py-2 bg-alt side-div">
          <!-- sidebar content -->
          <h1>Topics</h1>
          <input type="text" v-model="filter" list="titles" placeholder="search" />
          <datalist id="titles">
            <option v-for="topic in topics" v-bind:key="topic.title">{{ topic.title }}</option>
          </datalist>
          <div
            class="card card-default my-2"
            v-for="topic in orderBy(filterBy(topics, filter), sortAttribute)"
            v-bind:key="topic.id"
          >
            <div class="card-img-top card-img-top-200 card-zoom">
              <router-link :to="`/topics/${topic.id}`">
                <img v-bind:src="topic.image_url" class="mx-auto img-fluid" />
              </router-link>
            </div>
            <div class="card-body py-2">
              <h6 class="small text-wide"></h6>
              <h5 class="card-title">
                <router-link :to="`/topics/${topic.id}`">
                  <b>{{ topic.title }}</b>
                </router-link>
              </h5>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.topic-img {
  width: 400px;
}
.card-image {
  width: 200px;
}
.side-div {
  background: #f6f7f7;
  padding-left: 50px;
}
.container {
  background: #ffffff;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
import moment from "moment";

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
    this.indexPosts();
  },

  methods: {
    relativeDate: function(date) {
      return moment(date).format("MMM D");
    },
    indexTopics: function() {
      axios.get("/api/topics").then(response => {
        console.log(response.data);
        this.topics = response.data.topics;
        this.posts = response.data.posts;
      });
    },
    indexPosts: function() {
      axios.get("/api/posts").then(response => {
        console.log(response.data);
        this.posts = response.data.posts;
      });
    },
  },
};
</script>
