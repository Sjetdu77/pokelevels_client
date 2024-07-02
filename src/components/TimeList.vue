<template>
    <div id="poke-time-list">
        <button v-if="time == 'a'" class="poke-time-button active">Tout temps</button>
        <button v-else class="poke-time-button inactive" @click="$emit('setTime', 'a')">Tout temps</button>
        <button v-if="time == 'm'" class="poke-time-button active">Matin</button>
        <button v-else class="poke-time-button inactive" @click="$emit('setTime', 'm')">Matin</button>
        <button v-if="time == 'd'" class="poke-time-button active">Jour</button>
        <button v-else class="poke-time-button inactive" @click="$emit('setTime', 'd')">Jour</button>
        <button v-if="time == 'n'" class="poke-time-button active">Nuit</button>
        <button v-else class="poke-time-button inactive" @click="$emit('setTime', 'n')">Nuit</button>
    </div>
</template>

<script>
export default {
    name: 'TimeList',
    props: ['time'],
    emits: ['setTime']
}
</script>

<style>
#poke-time-list {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 75%;
    height: 5vh;
    background-color: #99f;
    display: flex;
    justify-content: space-between;
}

.poke-time-button {
    margin: 5px;
    width: 20%;
    border-radius: 1.5vw;
    font-size: 1vw;
}
</style>