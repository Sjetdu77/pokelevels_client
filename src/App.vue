<template>
  <div class="App">
    <header>
      <PokeHeader />
    </header>

    <main>
      <GameList @setActive="onSetActive" :games="games" :active="active" />
      <MainComponent @setAccess="e => { actual = e }" :active="active" :accesses="accesses" :actual="actual" :routes="routes" />
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
      active: {},
      axios,
      accesses: [],
      actual: 0,
      routes: []
    }
  },
  mounted() {
    axios.get("http://127.0.0.1:8000/api/games/").then(res => {
      this.games = res.data;
    })
  },
  methods: {
    async onSetActive(game) {
      const accesses = []

      for (const accessURL of game.accesses) {
        const proxy = await axios.get(accessURL)
        accesses.push(proxy.data)
      }

      accesses.sort((a, b) => {
        return a.number - b.number;
      })

      if (this.active.id) {
        const proxy = await axios.get(`http://127.0.0.1:8000/api/routes_from_game/${this.active.id}/`)
        this.routes = proxy.data
      }

      this.accesses = accesses
      this.active = game;
      this.actual = 0;
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

.inactive {
  background-image: linear-gradient(#f77, white);
}

.active {
  background-image: linear-gradient(#77f, white);
}
</style>
