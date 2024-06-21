<template>
  <div class="App">
    <header>
      <PokeHeader />
    </header>

    <main>
      <GameList @setActive="onSetActive" :games="games" :active="active" />
      <MainComponent :active="active" />
    </main>
  </div>
</template>

<script>
import GameList from './components/GameList.vue';
import PokeHeader from './components/PokeHeader.vue';
import MainComponent from './components/MainComponent.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    PokeHeader,
    GameList,
    MainComponent
  },
  data() {
    return {
      games: [],
      active: {}
    }
  },
  mounted() {
    axios.get("http://127.0.0.1:8000/api/games/").then(res => {
      this.games = res.data;
    })
  },
  methods: {
    onSetActive(game) {
      this.active = game;
      console.log(this.active)
    }
  }
}
</script>

<style>
.App {
  text-align: center;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva,
    Verdana, sans-serif;
}
</style>
