<script setup>
defineProps({
    id: String,
    disabled: Boolean
})
</script>

<template>
    <div class="offcanvasOption" :class="{ disabled: disabled }">
        <slot />
    </div>
</template>

<style scoped>
.offcanvasOption {
    margin-top: 1.5vh;
    padding: calc(0.25rem + 0.25vh) calc(0.75rem + 0.75vh);

    font-size: calc(0.875rem + 0.875vh) !important;
    font-weight: 400;

    border: 0.1em solid rgba(255, 255, 255, 0.25);
    border-radius: 0.5em;

    background-color: rgb(73, 0, 133);

    cursor: pointer;
}
.offcanvasOption:is(.disabled) {
    background-color: rgb(36, 36, 36);

    opacity: 0.5;
}
.offcanvasOption:not(.disabled):hover {
    background-color: rgb(100, 0, 182);
}
</style>