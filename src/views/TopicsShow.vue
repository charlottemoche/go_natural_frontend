<template>
  <div class="topics-show">
    <div class="carousel slide w-100 mb-5" data-ride="carousel" data-interval="10000" id="postsCarousel">
      <div class="container mt-2 px-0">
        <br />
        <h1>{{ topic.title }}</h1>
        <hr />
        <div class="carousel-inner pb-2">
          <!-- three (3) sildes per carousel item -->
          <div class="carousel-item active flex-column flex-sm-row align-items-stretch">
            <div class="col-md-4" v-for="post in topic.posts" v-bind:key="post.id">
              <div class="card card-default h-100">
                <div class="card-img-top-200 card-zoom">
                  <router-link :to="`/posts/${post.id}`">
                    <span>
                      <img v-bind:src="post.image_url" alt="" />
                    </span>
                  </router-link>
                </div>
                <div class="card-body pt-2 pl-1">
                  <h6 class="small text-wide text-truncate pb-1">Blog Post</h6>
                  <h2>
                    <a href="./blog-detail.html">{{ post.title }}</a>
                  </h2>
                  <h8>{{ post.subtitle }}</h8>
                  <br />
                  <small>
                    by
                    <b>{{ post.user.name }}</b>
                  </small>
                  <br />
                  <small>{{ relativeDate(post.created_at) }}</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
img {
  width: 340px;
}
</style>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function() {
    return {
      topic: {},
      post: {
        user: {
          name: "",
        },
      },
    };
  },
  created: function() {
    axios.get(`/api/topics/${this.$route.params.id}`).then(response => {
      this.topic = response.data;
      console.log(this.topic);
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).format("MMM D");
    },
  },
};
</script>
