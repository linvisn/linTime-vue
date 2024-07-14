<script setup>
import Timer from "@/components/Functional/Timer.vue"
import Stopwatch from "@/components/Functional/Stopwatch.vue"
import Tabata from "@/components/Functional/Tabata.vue"

defineProps({
    id: String,
    title: String,
    active: Boolean
})
</script>

<template>
    <div class="main-panel" :class="{ 'main-panel-active': active }" :id="id">
        <div class="MPHeader sticky-top">{{ title }}</div>
        <Timer v-if="title === 'Timer'" />
        <Stopwatch v-if="title === 'Stopwatch'" />
        <Tabata v-if="title === 'Tabata'" />
    </div>
</template>

<style scoped>
.main-panel {
    position: absolute;
    left: 0;

    height: 100%;
    width: 100%;

    font-size: calc(1.25rem + 0.75vh);

    background-color: rgb(59, 59, 59) !important;

    overflow: auto;
}
.main-panel-active {
    z-index: 1;
}

.MPHeader {
    padding: 1vmax 3vmax;

    font-size: calc(1.5rem + 1vh);
    font-weight: 700;

    text-shadow: 0 0 0.25em black;

    background: linear-gradient(rgb(59, 59, 59), transparent);

    z-index: inherit;
}
</style>