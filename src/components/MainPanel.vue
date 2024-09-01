<script setup>
import Timer from "@/components/Functional/Timer.vue"
import Stopwatch from "@/components/Functional/Stopwatch.vue"
import Tabata from "@/components/Functional/Tabata.vue"

defineProps({
    id: String,
    title: String,
    active: Boolean
})
const isStarted = defineModel('isStarted')
</script>

<template>
    <div class="main-panel" :class="{ 'main-panel-active': active }" :id="id">
        <div class="MPHeader sticky-top">{{ title }}</div>
        <Timer v-if="title === 'Timer'" v-model:isStarted="isStarted" />
        <Stopwatch v-if="title === 'Stopwatch'" v-model:isStarted="isStarted" />
        <Tabata v-if="title === 'Tabata'" v-model:isStarted="isStarted" />
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
.midnight-fireplace {
  background: linear-gradient(64deg, rgb(14, 3, 116), rgb(105, 56, 0));
}
.mocha {
    background-color: rgb(var(--ctp-mocha-mantle-rgb)) !important;
}

.MPHeader {
    padding: calc(0.5rem + 1.25vh) calc(0.75rem + 2.25vh);

    font-size: calc(1.5rem + 1vh);
    font-weight: 700;

    text-shadow: 0 0 0.25em black;

    background: linear-gradient(rgb(59, 59, 59), transparent);

    z-index: inherit;
}
.midnight-fireplace .MPHeader {
    background: linear-gradient(rgb(97, 55, 0), transparent);
}
.mocha .MPHeader {
    background: linear-gradient(rgb(var(--ctp-mocha-base-rgb)), transparent) !important;
}
</style>