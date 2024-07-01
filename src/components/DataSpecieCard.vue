<template>
    <div class="poke-specie-card" v-if="specie && wild">
        <div class="poke-specie-card-name">{{ specie.name }}</div>
        <div class="poke-specie-card-level">(niveau {{ wild.lvl }})</div>
        <img class="poke-specie-card-img" :src="require(`../${specie.sprite}`)" :alt="specie.name" />
        <div>{{ wild.probability }} % ({{ xp }} XP)</div>
    </div>
</template>

<script>
export default {
    name: "DataSpecieCard",
    props: ['wild', 'specie', 'xp']
}
</script>

<style>
.poke-specie-card {
    width: 10vw;
    background-image: linear-gradient(#f77, white);
}

.poke-specie-card-name {
    font-size: 1.2vw;
}

.poke-specie-card-level {
    font-size: 1vw;
}

.poke-specie-card-img {
    width: 9vw;
}
</style>