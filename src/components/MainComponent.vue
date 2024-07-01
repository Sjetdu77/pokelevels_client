<template>
    <div id="poke-main">
        <div id="poke-level-min">
            <div>Niveau maximum souhaité :</div>
            <input type="number" min="1" max="100" :value="level" @input="$emit('setLevel', $event.target.value)" />
        </div>
        <DataRoute :route="route" :wilds="wilds" :species="species" :allXP="allXP" :mean="mean" />
        <div id="poke-access-list">
            <AccessButton v-for="access of accesses" :key="access.id" :access="access" :actual="actual" @setAccess="$emit('setAccess', access.number)" />
        </div>
    </div>
</template>

<script>
import AccessButton from './AccessButton.vue';
import DataRoute from './DataRoute.vue';

export default {
    name: 'MainComponent',
    props: ['active', 'accesses', 'actual', 'route', 'wilds', 'species', 'level', 'allXP', 'mean'],
    emit: ['setAccess', 'setLevel'],
    components: { AccessButton, DataRoute }
}
</script>

<style>
#poke-main {
    position: absolute;
    background-color: #eee;
    bottom: 0;
    right: 0;
    width: calc(75% - 32px);
    height: calc(87.5vh - 20px);
}

#poke-level-min {
    position: absolute;
    display: flex;
    top: 0;
    width: 100%;
    height: 3vh;
    background-color: #ddd;
    font-size: 2.5vh;
}

#poke-access-list {
    position: absolute;
    right: 0;
    height: 100%;
    background-color: #fdd;
    width: 25%;
}
</style>