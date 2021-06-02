<template>
  <div class="photos-new">
    <h1>New Photo</h1>
    <form v-on:submit.prevent="createPhoto()">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="newPhoto.name" />
      Width:
      <input type="text" v-model="newPhoto.width" />
      Height:
      <input type="text" v-model="newPhoto.height" />
      Url:
      <input type="text" v-model="newPhoto.url" />
      <input type="submit" value="Create" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newPhoto: {},
      errors: [],
    };
  },
  created: function () {},
  methods: {
    createPhoto: function () {
      var params = {
        name: this.newPhoto.name,
        width: this.newPhoto.width,
        height: this.newPhoto.height,
        url: this.newPhoto.url,
      };
      axios
        .post("/photos", params)
        .then((response) => {
          console.log("photos create", response);
          this.$router.push("/photos");
        })
        .catch((error) => {
          console.log("photos create error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
