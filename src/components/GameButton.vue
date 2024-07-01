<script>
export default {
  name: 'GameButton',
  props: ['game', 'active'],
  emit: ['setActive']
}
</script>

<template>
  <button v-if="game.name == active.name" class="poke-list-button active">
    <div class='poke-list-button-title' v-if="game.name.startsWith(`Légendes`)" :style="{ fontWeight: `bold` }">Légendes Pokémon : {{ game.name.slice(11) }}</div>
    <div class='poke-list-button-title' v-else :style="{ fontWeight: `bold` }">Pokémon {{ game.name }}</div>
    <div class='poke-list-button-circle' :style="{ backgroundColor: game.color }">
      <img class='poke-list-button-image' src="../assets/pokegame.png" alt="{{ game.name }}" />
    </div>
  </button>
  <button v-else class="poke-list-button inactive" @click="$emit('setActive')">
    <div class='poke-list-button-title' v-if="game.name.startsWith(`Légendes`)" :style="{ fontWeight: `bold` }">Légendes Pokémon : {{ game.name.slice(11) }}</div>
    <div class='poke-list-button-title' v-else :style="{ fontWeight: `bold` }">Pokémon {{ game.name }}</div>
    <div class='poke-list-button-circle' :style="{ backgroundColor: game.color }">
      <img class='poke-list-button-image' src="../assets/pokegame.png" alt="{{ game.name }}" />
    </div>
  </button>
</template>

<style>
.poke-list-button {
  text-align: left;
  border-radius: 1.5vw;
  font-size: 1.2vw;
  margin-bottom: 5px;
  margin-top: 5px;
  padding-right: 0;
  padding-top: 0;
  display: flex;
  justify-content: space-between;
  min-height: 3vw;
}

.poke-list-button-circle {
  max-height: 100%;
  aspect-ratio: 1 / 1;
  border-radius: 50%;
  overflow-y: hidden;
}

.poke-list-button-image {
  width: 100%;
  height: 100%;
}

.poke-list-button-title {
  transform: translateY(-50%);
  -ms-transform: translateY(-50%);
  position: relative;
  top: 50%;
  margin: 0;
}
</style>
