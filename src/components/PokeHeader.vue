<script>
import logo from '../assets/logo.png'

export default {
  data() {
    return { logo }
  }
}
</script>

<template>
  <div className="poke-title">
    <span>PokéLevels</span>
    <img :src="logo" alt="PokéLevels logo" id="poke-logo" />
  </div>
</template>

<style>
.poke-title {
  top: 0;
  left: 0;
  display: flex;
  justify-content: right;
  padding: 10px;
  background-color: #f77;
  position: absolute;
  width: calc(100% - 20px);
  z-index: 50;
}

.poke-title > span {
  font-size: 3em;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
}

#poke-logo {
  margin-left: 1%;
  height: 12.5vh;
}
</style>
