<template>
    <Dialog v-model:visible="visible" maximizable footer="Close" header="Menu 1" :style="{ width: '50vw' }">
        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
            Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
    </Dialog>
</template>

<script setup>
import { ref, watch } from 'vue';

const emit=defineEmits([
'closeModal'
])
const visible=ref(false);
const props = defineProps({
	propsVisible:Boolean,	
})

watch(props,(v)=>{
    visible.value=v.propsVisible;
})

watch(visible,(v)=>{
  emit('closeModal',v)
})
</script>
