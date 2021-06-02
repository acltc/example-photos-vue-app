<template>
  <div class="home">
    <h1>New Photo</h1>
    <div>
      Name:
      <input type="text" v-model="newPhoto.name" />
      Width:
      <input type="text" v-model="newPhoto.width" />
      Height:
      <input type="text" v-model="newPhoto.height" />
      Url:
      <input type="text" v-model="newPhoto.url" />
      <button v-on:click="createPhoto()">Create Photo</button>
    </div>
    <h1>All Photos</h1>
    <div v-for="photo in photos" v-bind:key="photo.id">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" v-bind:alt="photo.name" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      photos: [],
      newPhoto: {},
    };
  },
  created: function () {
    this.indexPhotos();
  },
  methods: {
    indexPhotos: function () {
      axios.get("/photos").then((response) => {
        console.log("photos index", response);
        this.photos = response.data;
      });
    },
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
          this.photos.push(response.data);
          this.newPhoto = {};
        })
        .catch((error) => {
          console.log("photos create error", error.response);
        });
    },
  },
};
</script>
