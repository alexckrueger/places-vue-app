<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "PLACES!",
      places: [],
      newPlace: {},
      currentPlace: {},
      errors: [],
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
      this.currentPlace = place;
      document.querySelector("#show-place").showModal();
    },
    updatePlace: function (place) {
      axios
        .post(`/places/${place.id}`, place)
        .then((response) => console.log(response.data))
        .catch((error) => console.log(error.response));
    },
    createPlace: function () {
      this.errors = [];
      axios
        .post("/places", this.newPlace)
        .then((response) => {
          console.log(response.data);
          this.places.push(this.newPlace);
          this.newPlace = {};
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    deletePlace: function (place) {
      axios.delete(`/places/${place.id}`).then((response) => console.log(response.data));
      this.places.splice(this.places.indexOf(place), 1);
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <p>
        Name:
        <input type="text" v-model="newPlace.name" />
      </p>
      <p>
        Address:
        <input type="text" v-model="newPlace.address" />
      </p>
      <button v-on:click="createPlace">Create Place</button>
      <p v-for="error in errors" v-bind:key="error" id="red">{{ error }}</p>
    </div>
    <div v-for="place in places" v-bind:key="place.id">
      <h3>{{ place.name }}</h3>
      <p>{{ place.address }}</p>
      <button v-on:click="showPlace(place)">More Info</button>
    </div>
    <dialog id="show-place">
      <form method="dialog">
        <h3>{{ currentPlace.name }}</h3>
        <p>{{ currentPlace.address }}</p>
        <h4>EDIT PLACE:</h4>
        <p>
          Name:
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="currentPlace.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">SAVE PLACE</button>
        <button v-on:click="deletePlace(currentPlace)" id="red">DELETE PLACE</button>
        <br />
        <button id="smol">CLOSE</button>
      </form>
    </dialog>
  </div>
</template>

<style>
#red {
  color: red;
  font-size: x-large;
}
#smol {
  font-size: xx-small;
}
</style>
