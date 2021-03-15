<template>
  <div class="users-edit">
    <form v-on:submit.prevent="updateUser()">
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      Profile:
      <div class="form-group">
        <label>Photo:</label>
        <input type="text" class="form-control" v-model="user.image_url" />
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
    };
  },
  created: function() {
    axios.get(`/api/users/${this.$route.params.id}`).then(response => {
      this.user = response.data;
      console.log(this.user);
    });
  },
  methods: {
    updateUser: function() {
      var params = {
        image_url: this.user.image_url,
        name: this.user.name,
        bio: this.user.bio,
      };
      axios
        .patch(`/api/users/${this.user.id}`, params)
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
