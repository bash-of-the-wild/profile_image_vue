<template>
  <div class="home">
    <form v-on:submit.prevent="submit()">
      <div>
        Name: <input v-model="newName">
      </div>
      <div>
        Avatar: <input type="file" v-on:change="setFile($event)" ref="fileInput">
      </div>
      <input type="submit" value="Go">
    </form>

    <div v-for="profile in profiles">
      <h2>{{ profile.name }}</h2>
      <img v-bind:src="profile.avatar" alt="">
    </div>
  </div>
</template>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      profiles: [],
      newName: "",
      image: ""
    };
  },
  created: function() {
    axios
      .get("http://localhost:3000/profiles")
      .then(response => {
        this.profiles = response.data;
      });
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    submit: function() {
      var params = new FormData();
      params.append("name", this.newName);
      params.append("avatar", this.image);

      axios
        .post("http://localhost:3000/profiles", params)
        .then(response => {
          this.newName = "";
          this.image = "";
          this.$refs.fileInput.value = "";
          this.profiles = response.data;
        });
    }
  }
}
</script>
