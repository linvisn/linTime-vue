<script setup>
defineProps({
    id: String,
    title: String,
    icon: String,
    prevOffcanvas: String
})
</script>

<template>
    <div class="offcanvas offcanvas-start" :id="id">
      <div class="offcanvas-header">
        <button class="offcanvas-prev" v-if="prevOffcanvas" data-bs-toggle="offcanvas" :href="'#' + prevOffcanvas" :aria-controls="prevOffcanvas"><i class="bi bi-arrow-left-circle-fill"></i></button>
        <span class="offcanvas-title"><i class="bi" :class="icon"></i> {{ title }}</span>
        <button class="offcanvas-close" data-bs-dismiss="offcanvas"><i class="bi bi-x-circle-fill"></i></button>
      </div>
      <div class="offcanvas-body">
        <slot />
      </div>
    </div>
</template>

<style scoped>
.offcanvas {
    min-width: 25%;

    transition: all 0.25s ease !important;
}

.offcanvas-header {
    display: flex;
    justify-content: space-between;

    padding: calc(0.75rem + 0.75vh) calc(1rem + 1vh);

    text-shadow: 0 0 1em black;

    background: linear-gradient(rgba(0, 0, 0, 0.25), transparent);
}

.offcanvas-title {
    font-size: calc(1rem + 1.35vh);
    font-weight: 700;
    text-align: center;
}


.offcanvas-body {
    padding: 0 calc(1.25rem + 1.25vh);
}

#settingsOffcanvas, #aboutOffcanvas, #customizationOffcanvas, #toolsOffcanvas {
    border-right: 0.1em solid rgba(255, 255, 255, 0.5);
}
#settingsOffcanvas {
    background: rgb(31, 31, 78) !important;
}
#aboutOffcanvas {
    text-align: center !important;

    background: rgb(78, 71, 31) !important;
}
#customizationOffcanvas {
    background: rgb(110, 67, 26) !important;
}
#toolsOffcanvas {
    background: rgb(102, 95, 30) !important;
}


.offcanvas-close, .offcanvas-prev {
    width: fit-content !important;
    height: fit-content !important;

    font-size: calc(0.75rem + 1.75vh) !important;

    border: 0 !important;
    outline: 0 !important;

    background: transparent !important;
    
    opacity: 0.75 !important;
}
.offcanvas-close i, .offcanvas-prev i {
    color: rgb(255, 255, 255) !important;
}
.offcanvas-close:hover, .offcanvas-prev:hover {
    opacity: 1 !important;
}
</style>