<template>
    <div id="poke-main"></div>
</template>

<script>
export default {
    name: 'MainComponent',
    props: ['active']
}
</script>

<style>
#poke-main {
    position: absolute;
    background-color: aqua;
    bottom: 0;
    right: 0;
    width: calc(75% - 32px);
    height: calc(87.5vh - 20px);
}
</style>