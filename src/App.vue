<template>
  <div class="App">
    <header>
      <PokeHeader />
    </header>

    <main>
      <GameList @setActive="onSetActive" :games="games" :active="active" />
      <div id="poke-main" v-if="route && wilds && species">
        <MainComponent @setLevel="setLevel" :route="route" :wilds="wilds" :species="species"
          :level="level" :allXP="allXP" :mean="mean" :mode="mode" />
        <TimeList @setTime="setTime" :time="time" />
        <AccessList @setAccess="setAccess" :accesses="accesses" :actual="actual" />
      </div>
      <div id="poke-choose" v-else-if="!active.id">Choisissez un jeu</div>
    </main>
  </div>
</template>

<script>
import GameList from './components/GameList.vue';
import PokeHeader from './components/PokeHeader.vue';
import MainComponent from './components/MainComponent.vue';
import AccessList from './components/AccessList.vue';
import TimeList from './components/TimeList.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    PokeHeader,
    GameList,
    MainComponent,
    AccessList,
    TimeList
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
      mode: '',
      species: {},
      level: 100,
      time: 'a'
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
      this.setRoute();
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

      this.setAccess(accesses.length - 1);
    },

    setAccess(num) {
      this.actual = num;
      this.setRoute();
    },

    setTime(t) {
      this.time = t;
      this.setRoute();
    },
    
    async setRoute() {
      const routes = [];
      for (const route of this.allRoutes) {
        if (route.access <= this.actual) {
          routes.push(route);
        }
      }

      const wildsProxy = await axios.get(`http://127.0.0.1:8000/api/wilds/`);
      const allWilds = wildsProxy.data;

      let bestSum = 0, bestRoute, goodWilds, goodSpecies = {}, generation = this.active.generation, goodXP = {}, goodMode = '';
      let allSpecies = {};
      for (const route of routes) {
        for (const mode of ['gs', 'sf']) {
          let sum = 0, species = {}, wilds_here = [], allXP = [];
          const wilds = route.wilds;
          for (const wildURL of wilds) {
            const wild = allWilds[wildURL.split(' ')[0]];

            if (this.time == 'a' || wild.time == 'a' || this.time == wild.time) {
              if (wild.lvl <= this.level) {
                if (wild.mode == mode) {
                  if (!allSpecies[wild.specie]) {
                    const specieProxy = await axios.get(`http://127.0.0.1:8000/api/species/${wild.specie}/`);
                    allSpecies[wild.specie] = specieProxy.data;
                  }

                  wilds_here.push(wild);

                  let specie = allSpecies[wild.specie];
                  species[wild.specie] = allSpecies[wild.specie];

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
          }

          if (sum > bestSum) {
            bestSum = sum;
            bestRoute = route;
            goodWilds = wilds_here;
            goodSpecies = species;
            goodXP = allXP;
            goodMode = mode;
          }
        }
      }

      this.route = bestRoute;
      this.wilds = goodWilds;
      this.species = goodSpecies;
      this.allXP = goodXP;
      this.mean = Math.round(bestSum);
      this.mode = goodMode;
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

#poke-main {
    position: absolute;
    background-color: #eee;
    bottom: 0;
    right: 0;
    width: calc(75% - 32px);
    height: calc(87.5vh - 20px);
}
</style>
