<template>
  <div class="users-edit">
    <form v-on:submit.prevent="updateUser()">
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Image:</label>
        <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput" />
      </div>
      <div class="form-group">
        <label>Name:</label>
        <input type="text" class="form-control" v-model="user.name" />
      </div>
      <div class="form-group">
        <label>Bio:</label>
        <input type="text" class="form-control" v-model="user.bio" />
      </div>
      <br />

      <input type="submit" class="btn btn-primary" value="Update Profile" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      user: {
        email: "",
      },
      errors: [],
      password: "",
      passwordConfirmation: "",
      imageFile: "",
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      this.user = response.data;
      console.log(this.user);
    });
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.imageFile = event.target.files[0];
      }
    },
    updateUser: function() {
      var formData = new FormData();
      formData.append("name", this.user.name);
      formData.append("bio", this.user.bio);
      if (this.imageFile) {
        formData.append("image_file", this.imageFile);
      }
      axios
        .patch(`/api/users/${this.user.id}`, formData)
        .then(response => {
          console.log(response.data);
          this.$router.push(`/users/${this.user.id}`);
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
