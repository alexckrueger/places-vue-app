<script>import axios from "axios";

export default {
  data: function () {
    return {
      message: "PLACES!",
      places: [],
      newPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/places").then((response) => (this.places = response.data));
    },
    showPlace: function (place) {
      console.log(place);
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>{{ places }}</p>
    <div v-for="place in places" v-bind:key="place.id">
      <h3>{{ place.name }}</h3>
      <p>{{ place.address }}</p>
      <button v-on:click="showPlace(place)">More Info</button>
    </div>
  </div>
</template>

<style></style>
