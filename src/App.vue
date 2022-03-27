<template>
  <v-app>
    <NavBar />
    <v-main>
      <v-card class="mx-auto" max-width="300" v-if="comic && loader === false">
        <v-img :src="comic.img" height="250px"></v-img>

        <v-card-title> {{ comic.title }} ( {{ comic.num }} ) </v-card-title>

        <v-card-subtitle> Year: {{ comic.year }} </v-card-subtitle>

        <v-card-text class="text--primary">
          <div>Month: {{ comic.month }} / Day: {{ comic.day }}</div>
        </v-card-text>

        <div class="text-center" @click="showAlert">
          <span bold>Rate comic!</span>
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
          <v-tooltip top>
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="blue" text v-on:click="randomComic()" v-bind="attrs" v-on="on">
                Random Comic
              </v-btn>
            </template>
            <span>Press for a new comic</span>
          </v-tooltip>              

          <v-spacer></v-spacer>
          <v-tooltip top>
            <template v-slot:activator="{ on, attrs }">
              <v-btn icon @click="show = !show" v-bind="attrs" v-on="on" color="blue">
              <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
            </v-btn>
            </template>
            <span>More information</span>
          </v-tooltip>
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
      <v-sheet class="pa-3" v-else>
        <v-skeleton-loader
          class="mx-auto"
          max-width="300"
          type="card"
        ></v-skeleton-loader>
      </v-sheet>
    </v-main>
  </v-app>
</template>

<script>
import NavBar from "./components/NavBar";
import Swal from "sweetalert2";

export default {
  name: "App",
  components: {
    NavBar,
  },
  inject: {
    theme: {
      default: { isDark: false },
    },
  },
  data() {
    return {
      comic: null,
      comicId: Math.floor(Math.random() * 2190 + 1).toString(),
      show: false,
      rating: 0,
      loader: false,
    };
  },
  mounted() {
    this._getComic();
  },
  methods: {
    randomComic() {
      this._resetValues();
      this._getComic();
    },
    async _getComic() {
      this.loader = true;
      await fetch(`https://xkcd.com/${this.comicId}/info.0.json`)
        .then((response) => response.json())
        .then((data) => (this.comic = data));
      this.loader = false;
    },
    _resetValues() {
      this.comicId = Math.floor(Math.random() * 1000 + 1).toString();
      this.rating = 0;
    },
    showAlert: function () {
      Swal.fire({
        position: "top",
        icon: "info",
        title: `Comic Stars: ${this.rating}`,
        showConfirmButton: false,
        timer: 3500,
        toast: true,
      });
    },
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
