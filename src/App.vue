<template>
  <v-app>
    <NavBar />
    <v-main>
      <v-card class="mx-auto" max-width="300">
        <v-img :src="comic.img" height="250px"></v-img>

        <v-card-title> {{ comic.title }} ( {{ comic.num }} ) </v-card-title>

        <v-card-subtitle> Year: {{ comic.year }} </v-card-subtitle>

        <v-card-text class="text--primary">
          <div>Month: {{ comic.month }} / Day: {{ comic.day }}</div>
        </v-card-text>

        <div class="text-center" @click="showAlert">
          <span bold>Califica el comic!</span>
          <v-rating
            v-model="rating"
            color="blue"
            background-color="grey darken-1"
            empty-icon="$ratingFull"
            half-increments
            hover
            large
          ></v-rating>
        </div>

        <v-card-actions>
          <v-btn color="blue" text v-on:click="randomComic()">
            Random Comic
          </v-btn>

          <v-spacer></v-spacer>

          <v-btn icon @click="show = !show">
            <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
          </v-btn>
        </v-card-actions>

        <v-expand-transition>
          <div v-show="show">
            <v-divider></v-divider>

            <v-card-text>
              {{ comic.transcript }}
            </v-card-text>
          </div>
        </v-expand-transition>
      </v-card>
    </v-main>
  </v-app>
</template>

<script>
import NavBar from "./components/NavBar";
import Swal from 'sweetalert2'

export default {
  name: "App",
  components: {
    NavBar,
  },
  data() {
    return {
      comic: null,
      comicId: Math.floor(Math.random() * 2190 + 1).toString(),
      show: false,
      rating: 0
    };
  },
  mounted() {
    fetch(`https://xkcd.com/${this.comicId}/info.0.json`)
      .then((response) => response.json())
      .then((data) => (this.comic = data));
  },
  methods: {
    randomComic: function () {
      fetch(`https://xkcd.com/${this.comicId}/info.0.json`)
        .then((response) => response.json())
        .then((data) => (this.comic = data));
      this.resetValues();
    },
    resetValues: function () {
      this.comicId = Math.floor(Math.random() * 1000 + 1).toString();
      this.rating = 0;
    },
    showAlert: function () {
      Swal.fire({
        position: 'top-end',
        icon: 'info',
        title: `Comic Stars: ${ this.rating }`,
        showConfirmButton: false,
        timer: 1500
      })
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
