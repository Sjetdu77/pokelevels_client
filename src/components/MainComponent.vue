<template>
    <div id="poke-main">
        <div v-if="active.id">
            <div id="poke-level-min">
                <div>Niveau maximum souhaité :</div>
                <input type="number" min="1" max="100" :value="level" @input="levelsChange($event.target.value)" />
            </div>
            <div id="poke-access-list">
                <AccessButton v-for="access of accesses" :key="access.id" :access="access" :actual="actual" @setAccess="$emit('setAccess', access.number)" />
            </div>
        </div>
        <div id="poke-choose" v-else>Choisissez un jeu</div>
    </div>
</template>

<script>
import AccessButton from './AccessButton.vue';

export default {
    name: 'MainComponent',
    props: ['active', 'accesses', 'actual', 'routes'],
    emit: ['setAccess'],
    components: { AccessButton },
    data() {
        return {
            level: 100
        }
    },
    methods: {
        levelsChange(lvl) {
            this.level = lvl
        }
    }
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

#poke-choose {
    top: 50%;
    left: 50%;
    position: absolute;
    -ms-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    margin: 0;
}

#poke-level-min {
    position: absolute;
    display: flex;
    top: 0;
    width: 100%;
    background-color: #ddd;
}

#poke-access-list {
    position: absolute;
    right: 0;
    height: 100%;
    background-color: #fdd;
    width: 25%;
}
</style>