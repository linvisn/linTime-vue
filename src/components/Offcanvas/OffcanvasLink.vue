<script setup>
defineProps({
    link: String
})
</script>

<template>
    <a class="offcanvasLink" target="_blank" :href="link"><slot /></a>
</template>

<style scoped>
.offcanvasLink {
    font-weight: 700;
    color: rgb(138, 172, 216);
    text-decoration: underline !important;
}
.offcanvasLink:hover {
    color: rgb(169, 192, 223);
}
</style>