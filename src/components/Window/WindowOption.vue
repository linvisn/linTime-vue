<script setup>
defineProps({
    id: String,
    disabled: Boolean
})
defineEmits(['changeWindow'])
</script>

<template>
    <li class="windowOption" :class="{ disabled: disabled }" @click="$emit('changeWindow', id)">
        <slot />
    </li>
</template>

<style scoped>
.windowOption {
    margin-top: 1.5vh;
    padding: calc(0.25rem + 0.25vh) calc(0.75rem + 0.75vh);

    font-size: calc(0.875rem + 0.875vh) !important;
    font-weight: 400;

    border-radius: 0.5em;

    background-color: rgb(73, 0, 133);
}
.windowOption:is(.disabled) {
    background-color: rgb(36, 36, 36);

    opacity: 0.5;
}
.windowOption:not(.disabled):hover {
    background-color: rgb(100, 0, 182);
}
</style>