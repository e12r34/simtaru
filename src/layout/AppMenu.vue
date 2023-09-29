<script setup>
import { ref, onMounted } from 'vue';

// import AppMenuItem from './AppMenuItem.vue';


const emit=defineEmits(['openMenu1','toggleJalan'])
const tampilkanJalanWaisai=ref(false)

function openMenu1() {
    emit('openMenu1',true)
}

function toggleJalan() {
    tampilkanJalanWaisai.value=!tampilkanJalanWaisai.value
    emit('toggleJalan', tampilkanJalanWaisai.value)
}

// import { library } from '@fortawesome/fontawesome-free'

onMounted(() => {
    console.log(`the component is now mounted.`)
    // const allIcons = library.definitions
    // console.log('List of Font Awesome Icons:', Object.keys(allIcons))
})


</script>

<template>
    <ul class="layout-menu">
        <li class="layout-root-menuitem">
            <div class="layout-menuitem-root-text"> Jalan</div>
            <ul class="layout-submenu">
                <li>
                    <a @click="toggleJalan" tabindex="0">
                        <font-awesome-icon icon="fa fa-road" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Jalan Raya Waisai
                        </span>
                    </a>
                </li>
                <li>
                    <a @click="openMenu1()" tabindex="0">
                        <font-awesome-icon icon="fa fa-road" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Jalan Lukas Dailon
                        </span>
                    </a>
                </li>
                <li>
                    <a tabindex="0">
                        <font-awesome-icon icon="fa fa-road" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Jalan Waisai Warsamdin
                        </span>
                    </a>
                </li>
            </ul>
        </li>
        <li class="layout-root-menuitem">
            <div class="layout-menuitem-root-text"> Rumah Sakit</div>
            <ul class="layout-submenu">
                <li>
                    <a tabindex="0">
                        <font-awesome-icon icon="fa fa-hospital" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Jalan Waisai Warsamdin
                        </span>
                    </a>
                </li>
            </ul>
        </li>
        <li class="layout-root-menuitem">
            <div class="layout-menuitem-root-text"> Bandara</div>
            <ul class="layout-submenu">
                <li>
                    <a tabindex="0">
                        <!-- <i class="pi pi-directions layout-menuitem-icon">

                        </i> -->
                        <!-- <i class="fa fa-plane layout-menuitem-icon"></i> -->
                        <font-awesome-icon icon="fa fa-plane" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Bandara Marinda Raja Ampat (RJM)
                        </span>
                    </a>
                </li>
            </ul>
        </li>
        <li class="layout-root-menuitem">
            <div class="layout-menuitem-root-text"> Pelabuhan</div>
            <ul class="layout-submenu">
                <li>
                    <a tabindex="0">
                        <!-- <i class="pi pi-directions layout-menuitem-icon">

                        </i> -->
                        <!-- <i class="fa fa-plane layout-menuitem-icon"></i> -->
                        <font-awesome-icon icon="fa fa-anchor" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Pelabuhan Waisai
                        </span>
                    </a>
                </li>
            </ul>
        </li>
        <li class="layout-root-menuitem">
            <div class="layout-menuitem-root-text"> Hotel / Resort</div>
            <ul class="layout-submenu">
                <li>
                    <a tabindex="0">
                        <!-- <i class="pi pi-directions layout-menuitem-icon">

                        </i> -->
                        <!-- <i class="fa fa-plane layout-menuitem-icon"></i> -->
                        <font-awesome-icon icon="fa fa-hotel" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Afud Dive Resort
                        </span>
                    </a>
                </li>

                <li>
                    <a tabindex="0">
                        <!-- <i class="pi pi-directions layout-menuitem-icon">

                        </i> -->
                        <!-- <i class="fa fa-plane layout-menuitem-icon"></i> -->
                        <font-awesome-icon icon="fa fa-hotel" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Raja Ampat Homestay
                        </span>
                    </a>
                </li>

                <li>
                    <a tabindex="0">
                        <!-- <i class="pi pi-directions layout-menuitem-icon">

                        </i> -->
                        <!-- <i class="fa fa-plane layout-menuitem-icon"></i> -->
                        <font-awesome-icon icon="fa fa-hotel" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Wapnansan Frankies Home Stay
                        </span>
                    </a>
                </li>
            </ul>
        </li>
        <li class="layout-root-menuitem">
            <div class="layout-menuitem-root-text"> PENGATURAN</div>
            <ul class="layout-submenu">

                <li>
                    <a tabindex="0">
                        <font-awesome-icon icon="fa fa-map-marker" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Tambah Titik
                        </span>
                    </a>
                </li>

                <li >
                    <a tabindex="0">
                        <font-awesome-icon icon="fa fa-mountain" class="layout-menuitem-icon" />
                        <span class="layout-menuitem-text">
                            Tambah Legenda
                        </span>
                    </a>
                </li>
            </ul>
        </li>
        <!-- <template v-for="(item, i) in model" :key="item">
            <app-menu-item v-if="!item.separator" :item="item" :index="i"></app-menu-item>
            <li v-if="item.separator" class="menu-separator"></li>
        </template> -->
        
        <!-- <li>
            <a href="https://www.primefaces.org/primeblocks-vue/#/" target="_blank">
                <img src="/layout/images/banner-primeblocks.png" alt="Prime Blocks" class="w-full mt-3" />
            </a>
        </li> -->
    </ul>
</template>

<style lang="scss" scoped></style>
