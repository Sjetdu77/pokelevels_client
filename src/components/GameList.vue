<script>
import GameButton from './GameButton.vue';

export default {
  name: 'GameList',
  props: ['games', 'active'],
  emits: ['setActive'],
  components: {
    GameButton
  }
}
</script>

<template>
  <div className="game-list">
    <h2>Liste des jeux</h2>
    <GameButton
      v-for="game in games" :key="game.id" :game="game" :active="active"
      @setActive="() => { $emit('setActive', game) }" />
  </div>
</template>

<style>
.game-list {
  background-color: #ddd;
  color: red;
  padding: 16px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 25%;
  height: calc(100% - 32px);
  overflow-y: scroll;
  text-align: left;
  position: absolute;
  z-index: 100;
  top: 0;
  left: 0;
}

.game-list > h2 {
  font-size: 2.5vw;
}
</style>
