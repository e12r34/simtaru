<script setup>
import AppMenu from './AppMenu.vue';

const emit=defineEmits(['openMenu1','toggleJalan'])
function Menu1(isOpen) {
    emit('openMenu1',isOpen)
}

function toggleJalan(toggled) {
    emit('toggleJalan', toggled)
}
</script>

<template>
    <app-menu @open-menu1="(openMenu1)=>Menu1(openMenu1)" @toggle-jalan="(res)=>toggleJalan(res)"></app-menu>
</template>

<style lang="scss" scoped></style>
