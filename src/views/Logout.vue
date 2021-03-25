<template>
  <div class="logout"></div>
</template>

<script>
import axios from "axios";
import swal from "sweetalert";

export default {
  created: function() {
    delete axios.defaults.headers.common["Authorization"];
    localStorage.removeItem("jwt");
    localStorage.removeItem("user_id");
    swal("You are now logged out");
    this.$router.push("/");
  },
};
</script>
