<template>
    <div id="poke-access-list">
        <AccessButton v-for="access of accesses" :key="access.id" :access="access" :actual="actual" @setAccess="$emit('setAccess', access.number)" />
    </div>
</template>

<script>
import AccessButton from './AccessButton.vue';

export default {
    name: 'AccessList',
    props: ['accesses', 'actual'],
    emit: ['setAccess', 'setLevel'],
    components: { AccessButton }
}
</script>

<style>
#poke-access-list {
    position: absolute;
    right: 0;
    height: 100%;
    background-color: #fdd;
    width: 25%;
}
</style>