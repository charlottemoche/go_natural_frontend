<template>
  <div class="posts-new">
    <div class="jumbotron">
      <div class="card border-secondary">
        <div class="card-header">
          <h3 class="mb-0 my-2">New Post</h3>
        </div>
        <div class="card-body">
          <form class="form" role="form" autocomplete="off" v-on:submit.prevent="createPost()">
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Title:</label>
              <div class="col-lg-9">
                <input class="form-control" type="text" value="" id="title" name="title" v-model="title" />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Subtitle:</label>
              <div class="col-lg-9">
                <input class="form-control" type="text" value="" id="subtitle" name="title" v-model="subtitle" />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Body:</label>
              <div class="col-lg-9">
                <div id="froala-editor">
                  <froala id="edit" :tag="'textarea'" :config="config" v-model="body"></froala>
                </div>
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Add an Image Url:</label>
              <div class="col-lg-9">
                <input class="form-control" type="text" value="" id="image-url" name="image-url" v-model="imageUrl" />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Choose a Topic:</label>
              <div class="col-lg-9">
                <select
                  class="form-select form-select-lg mb-3"
                  aria-label="Default select example"
                  name="topics"
                  v-model="topicId"
                >
                  <option value="1">Product Alternatives</option>
                  <option value="2">Natural Cleaning</option>
                  <option value="3">Food Waste Reduction</option>
                  <option value="4">Water Conservation</option>
                </select>
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label"></label>
              <div class="col-lg-9">
                <input type="submit" class="btn btn-primary" value="Post" />
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.red-text {
  color: red;
}
.jumbotron {
  padding-left: 220px;
  padding-right: 220px;
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
