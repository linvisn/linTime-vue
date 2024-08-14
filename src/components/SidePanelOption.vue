<script setup>
defineProps({
    title: String,
    icon: String,
    active: Boolean,
    MP: String
})
defineEmits(['switchMPState'])
</script>

<template>
    <div class="sidePanelOption" :class="{ active: active }" @click="$emit('switchMPState', $event, MP)">
        <i class="bi" :class="icon"></i><span class="title d-none d-lg-inline">{{ title }}</span>
    </div>
</template>

<style scoped>
.sidePanelOption {
    display: flex;
    justify-content: center;

    margin-bottom: 1vh;
    padding: 0.25vmax 0;

    font-size: calc(0.75rem + 1vh) !important;
    font-weight: 500;
    text-align: center;

    border-radius: 0.25em;

    background: rgb(50, 73, 73);

    cursor: pointer;
    white-space: pre-wrap !important;
}
.sidePanelOption:hover {
    background-color: rgb(77, 95, 95);
}
.sidePanelOption:is(.active) {
    background-color: rgb(79, 116, 116);
}
.midnight-fireplace {
  background: rgb(98, 105, 0, 0.5);
}
.midnight-fireplace:hover {
  background: rgb(98, 105, 0, 0.75);
}
.midnight-fireplace:is(.active) {
  background: rgb(98, 105, 0, 0.875);
}

.title {
    margin-left: 0.25em;
}
</style>