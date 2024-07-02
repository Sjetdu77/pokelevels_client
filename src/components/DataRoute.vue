<template>
    <div id="poke-data-route" v-if="route">
        <div id="poke-data-title" v-if="mode == 'gs'">{{ route.name }}</div>
        <div id="poke-data-title" v-else-if="mode == 'sf'">{{ route.name }} - En surfant</div>
        <div id="poke-group-card">
            <DataSpecieCard v-for="wild of wilds" :key="wild.id" :specie="species[wild.specie]" :wild="wild" :xp="allXP[wild.id]" />
        </div>
        <div id="poke-data-mean">Moyenne de gain d'expérience : <span id="poke-data-xp">{{ mean }}</span> XP par pokémon vaincu</div>
    </div>
</template>

<script>
import DataSpecieCard from "./DataSpecieCard.vue"

export default {
    name: "DataRoute",
    props: ['route', 'wilds', 'species', 'allXP', 'mean', 'mode'],
    components: { DataSpecieCard }
}
</script>

<style>
#poke-data-route {
    position: absolute;
    background-color: #fff;
    bottom: 0;
    width: 75%;
    height: calc(84.5vh - 20px);
}

#poke-data-title {
    background-image: linear-gradient(#c0c, white);
    width: 60%;
    transform: translateX(30%);
    -ms-transform: translateX(30%);
    border: 0.3vh black solid;
    border-radius: 3vh;
    margin: 1vh;
    font-size: 1.5vw;
}

#poke-group-card {
    position: relative;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    overflow: clip;
    font-size: 1vw;
    height: calc(75vh - 20px - 2.5vw);
}

#poke-data-xp {
    font-weight: bold;
    color: red;
}
</style>