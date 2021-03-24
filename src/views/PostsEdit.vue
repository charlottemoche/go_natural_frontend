<template>
  <div class="posts-edit">
    <div class="jumbotron">
      <div class="card border-secondary">
        <div class="card-header">
          <h3 class="mb-0 my-2">Edit Post</h3>
        </div>
        <div class="card-body">
          <form class="form" role="form" autocomplete="off" v-on:submit.prevent="updatePost()">
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Title:</label>
              <div class="col-lg-9">
                <input class="form-control" type="text" value="" id="title" name="title" v-model="post.title" />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Subtitle:</label>
              <div class="col-lg-9">
                <input class="form-control" type="text" value="" id="subtitle" name="title" v-model="post.subtitle" />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Body:</label>
              <div class="col-lg-9">
                <div id="froala-editor">
                  <froala id="edit" :tag="'textarea'" :config="config" v-model="post.body"></froala>
                </div>
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Add an Image Url:</label>
              <div class="col-lg-9">
                <input
                  class="form-control"
                  type="text"
                  value=""
                  id="image-url"
                  name="image-url"
                  v-model="post.image_url"
                />
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label">Choose a Topic:</label>
              <div class="col-lg-9">
                <select
                  class="form-select form-select-lg mb-3"
                  aria-label="Default select example"
                  name="topics"
                  v-model="post.topic_id"
                >
                  <option value="1">Replacement Items</option>
                  <option value="2">Natural Cleaning</option>
                  <option value="3">Food Waste Reduction</option>
                </select>
              </div>
            </div>
            <div class="form-group row">
              <label class="col-lg-3 col-form-label form-control-label"></label>
              <div class="col-lg-9">
                <input type="submit" class="btn btn-primary" value="Update" />
                <button v-on:click="destroyPost()" class="btn btn-danger">Delete</button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
img {
  width: 350px;
}
.jumbotron {
  padding-left: 220px;
  padding-right: 220px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      post: {},
      errors: [],
      content: "",
      options: {},
      config: {
        events: {
          initialized: function() {
            console.log("initialized");
          },
        },
      },
    };
  },
  created: function() {
    axios.get(`/api/posts/${this.$route.params.id}`).then(response => {
      this.post = response.data;
      console.log(this.post);
    });
  },
  methods: {
    updatePost: function() {
      var params = {
        title: this.post.title,
        subtitle: this.post.subtitle,
        body: this.post.body,
        image_url: this.post.image_url,
        topic_id: this.post.topic_id,
      };
      axios
        .patch(`/api/posts/${this.post.id}`, params)
        .then(response => {
          console.log(response.data);
          this.$router.push(`/posts/${this.post.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyPost: function() {
      if (confirm("Are you sure?")) {
        axios.delete(`/api/posts/${this.post.id}`).then(response => {
          console.log(response.data);
          this.$router.push("/");
        });
      }
    },
  },
};
</script>
