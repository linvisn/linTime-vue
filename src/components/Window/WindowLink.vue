<script setup>
defineProps({
    link: String
})
</script>

<template>
    <a class="windowLink" target="_blank" :href="link"><slot /></a>
</template>

<style scoped>
.windowLink {
    font-weight: 700;
    color: rgb(138, 172, 216);
    text-decoration: underline !important;
}
.windowLink:hover {
    color: rgb(169, 192, 223);
}
</style>