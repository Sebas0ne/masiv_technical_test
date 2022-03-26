<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
        <div v-for="(value, key) in comic" v-bind:key="key">
          {{ key }}: {{ value }}
        </div>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data () {
    return {
      comic: null,
      comicId: Math.floor(Math.random() * 806 + 1).toString()
    }
  },
  mounted () {
    fetch(`https://xkcd.com/${this.comicId}/info.0.json`)
      .then(response => response.json())
      .then(data =>  this.comic = data);
  }
}
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
