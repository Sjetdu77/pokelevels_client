<template>
    <button v-if="access.number <= actual" class="poke-list-access-button active" @click="$emit('setAccess', access.number)">
        {{ access.name }}
    </button>
    <button v-else class="poke-list-access-button inactive" @click="$emit('setAccess', access.number)">
        {{ access.name }}
    </button>
</template>

<script>
export default {
    name: 'AccessButton',
    props: ['access', 'actual'],
    emit: ['setAccess']
}
</script>

<style>
.poke-list-access-button {
    text-align: left;
    border-radius: 15px;
    font-size: 1.2vw;
    margin-bottom: 5px;
    margin-top: 5px;
    padding-right: 0;
    padding-top: 0;
    display: flex;
    justify-content: space-between;
    min-height: 1vw;
    width: 100%;
}
</style>