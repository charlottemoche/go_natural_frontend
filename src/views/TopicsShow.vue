<template>
  <div class="topics-show">
    <div class="container mt-2 px-0">
      <br />
      <h1>{{ topic.title }}</h1>
      <br />

      <span class="anchor" id="card_feature"></span>
      <div class="card card-default">
        <div class="row">
          <div class="col-md-4" v-for="post in topic.posts" v-bind:key="post.id">
            <div class="card-img-top card-img-top-300 card-zoom">
              <router-link :to="`/posts/${post.id}`">
                <span>
                  <img v-bind:src="post.image_url" alt="" />
                </span>
              </router-link>
            </div>
            <div class="card-body pt-2">
              <h6 class="text-uppercase small">Blog Post</h6>
              <h3 class="card-title">{{ post.title }}</h3>
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
    <hr />
  </div>
</template>

<style scoped>
img {
  width: 360px;
  height: 260px;
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
