<script setup>
import AppMenu from './AppMenu.vue';

const emit=defineEmits(['openMenu1','toggleJalan','togglePelabuhan','togglePolygon','toggleWaisai'])
function Menu1(isOpen) {
    emit('openMenu1',isOpen)
}

function toggleJalan(toggled) {
    emit('toggleJalan', toggled)
}

function togglePelabuhan(toggled) {
    emit('togglePelabuhan', toggled)
}
function togglePolygon(toggled) {
    emit('togglePolygon', toggled)
}

function toggleWaisai(toggled) {
    emit('toggleWaisai', toggled)
}
</script>

<template>
    <app-menu 
        @open-menu1="(openMenu1)=>Menu1(openMenu1)" 
        @toggle-jalan="(res)=>toggleJalan(res)" 
        @toggle-pelabuhan="(res)=>togglePelabuhan(res)"
        @toggle-polygon="(res)=>togglePolygon(res)"
        @toggle-waisai="(res)=>toggleWaisai(res)"
    >
    </app-menu>
</template>

<style lang="scss" scoped></style>
