<template>
    <div id="poke-level-min">
        <div>Niveau maximum souhaité :</div>
        <input type="number" min="1" max="100" :value="level" @input="$emit('setLevel', $event.target.value)" />
    </div>
    <DataRoute :route="route" :wilds="wilds" :species="species" :allXP="allXP" :mean="mean" :mode="mode" />
</template>

<script>
import DataRoute from './DataRoute.vue';

export default {
    name: 'MainComponent',
    props: ['route', 'wilds', 'species', 'level', 'allXP', 'mean', 'mode'],
    emit: ['setAccess', 'setLevel'],
    components: { DataRoute }
}
</script>

<style>
#poke-level-min {
    position: absolute;
    display: flex;
    top: 0;
    width: 100%;
    height: 3vh;
    background-color: #ddd;
    font-size: 2.5vh;
}
</style>