<template>
  <div class="App">
    <header>
      <PokeHeader />
    </header>

    <main>
      <GameList @setActive="onSetActive" :games="games" :active="active" />
      <MainComponent v-if="route && wilds && species"
        @setAccess="setAccess" @setLevel="setLevel" :active="active" :accesses="accesses" :actual="actual"
        :route="route" :wilds="wilds" :species="species" :level="level" :allXP="allXP" :mean="mean" />
      <div id="poke-choose" v-else-if="!active.id">Choisissez un jeu</div>
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
      allRoutes: [],
      route: null,
      wilds: null,
      allXP: null,
      mean: null,
      species: {},
      level: 100
    }
  },
  mounted() {
    axios.get("http://127.0.0.1:8000/api/games/").then(res => {
      this.games = res.data;
    })
  },
  methods: {
    setLevel(lvl) {
      this.level = lvl;
      this.setRoutes();
    },

    async onSetActive(game) {
      const accesses = [];

      for (const accessURL of game.accesses) {
        const proxy = await axios.get(`http://127.0.0.1:8000/api/accesses/${accessURL}/`);
        accesses.push(proxy.data);
      }

      accesses.sort((a, b) => {
        return a.number - b.number;
      })

      this.accesses = accesses;
      this.active = game;

      if (this.active.id) {
        const proxy = await axios.get(`http://127.0.0.1:8000/api/routes_from_game/${this.active.id}/`);
        this.allRoutes = proxy.data;
      }

      this.setAccess(0);
    },

    async setAccess(num) {
      this.actual = num;
      this.setRoutes();
    },
    
    async setRoutes() {
      const routes = [];
      for (const route of this.allRoutes) {
        if (route.access <= this.actual) {
          routes.push(route);
        }
      }

      let bestSum = 0, bestRoute, goodWilds, goodSpecies = {}, generation = this.active.generation, goodXP = {};
      for (const route of routes) {
        let sum = 0, species = {}, wilds_here = [], allXP = [];
        const wilds = route.wilds;
        for (const wildURL of wilds) {
          const allWildWords = wildURL.split(' ');
          const wildProxy = await axios.get(`http://127.0.0.1:8000/api/wilds/${allWildWords[0]}/`);
          const wild = wildProxy.data;

          wilds_here.push(wild);

          if (wild.lvl <= this.level) {
            if (wild.mode == 'gs') {
              const specieProxy = await axios.get(`http://127.0.0.1:8000/api/species/${wild.specie}/`);
              const specie = specieProxy.data;

              species[wild.specie] = specie;

              let xp = generation > 4 ? specie.xp : specie.xp1_4;

              if (generation <= 4 || generation == 6) {
                sum += (wild.lvl * xp * wild.probability) / 700;
                allXP[wild.id] = Math.round((wild.lvl * xp) / 7);
              } else {
                sum += (wild.lvl * xp / 5 * Math.pow((2 * wild.lvl + 10) / (wild.lvl + 10), 2.5) + 1) * wild.probability / 100;
                allXP[wild.id] = Math.round(wild.lvl * xp / 5 * Math.pow((2 * wild.lvl + 10) / (wild.lvl + 10), 2.5) + 1);
              }
            }
          } else {
            sum = -10000;
          }
        }

        if (sum > bestSum) {
          bestSum = sum;
          bestRoute = route;
          goodWilds = wilds_here;
          goodSpecies = species;
          goodXP = allXP;
        }
      }

      this.route = bestRoute;
      this.wilds = goodWilds;
      this.species = goodSpecies;
      this.allXP = goodXP;
      this.mean = Math.round(bestSum);
    }
  }
}
</script>

<style>
.App {
  text-align: center;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva,
    Verdana, sans-serif;
  overflow: clip;
}

.inactive {
  background-image: linear-gradient(#f77, white);
}

.active {
  background-image: linear-gradient(#77f, white);
}

#poke-choose {
    top: 50%;
    left: 50%;
    position: absolute;
    margin: 0;
}
</style>
