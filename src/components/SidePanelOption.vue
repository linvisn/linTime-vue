<script setup>
defineProps({
    title: String,
    icon: String,
    active: Boolean,
    MP: String,
    window: String
})
defineEmits(['switchMPState', 'switchWindow'])
</script>

<template>
    <li class="sidePanelOption" :class="{ active: active }" @click="$emit('switchMPState', $event, MP), $emit('switchWindow', window)">
        <div>
            <i class="bi" :class="icon"></i><span class="title d-none d-lg-inline">{{ title }}</span>
        </div>
    </li>
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

    background-color: rgb(50, 73, 73);

    white-space: pre-wrap !important;
}
.sidePanelOption:hover {
    background-color: rgb(77, 95, 95);
}
.active {
    background-color: rgb(79, 116, 116);
}

.title {
    margin-left: 0.25em;
}
</style>