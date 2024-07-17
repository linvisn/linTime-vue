<script setup>
defineProps({
    id: Number,
    timeElapsed: String,
    timeElapsedFromStart: String,
    timeElapsedFromStartValue: Number
})
</script>

<template>
<div class="lap row">
    <div class="lap-left col-12 col-sm">
        <div>Lap {{ id }}</div>
        <div class="lap-left-time">Time elapsed: {{ timeElapsed }}</div>
    </div>

    <div class="lap-right col">
        {{ timeElapsedFromStart }}
    </div>
</div>
</template>

<style scoped>
.lap {
    margin: 0;
    margin-bottom: 1vmax;
    padding: calc(0.375rem + 0.375vh) calc(1.375rem + 1.375vh);

    border: 0.1vmax solid rgba(255, 255, 255, 0.5);
    border-right: none;
    border-left: none;
    border-radius: 0.5em;
}

.lap-left, lap-right {
    padding: 0 !important;
}

.lap-left {
    padding: 0 !important;

    font-size: calc(0.875rem + 0.875vh);
    font-weight: 600;
}
.lap-left-time {
    font-weight: 400;

    opacity: 0.75 !important;
}

.lap-right {
    display: flex;
    align-items: center;
    justify-content: flex-end;

    padding: 0 !important;

    font-size: calc(1rem + 1.25vh);
    font-weight: 700;
}
</style>