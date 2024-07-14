<script setup>
import { ref } from 'vue'
import Button from "@/components/Button.vue"
import Lap from '@/components/Functional/Lap.vue'
import stopwatchSound from '@/assets/stopwatchSound.mp3'

const ms = ref(0)
const isStarted = ref(false)
const isPaused = ref(false)

let startTime = 0
let elapsedTime = 0
let stopwatch = null
let lapsAmount = 0

const laps = ref([])


const formatTime = (time) => {
    return `${ String(Math.floor((time / 1000) / 60)).padStart(2, '0') }:${ String(Math.floor((time / 1000) % 60)).padStart(2, '0') }.${ String(time % 1000).padStart(3, '0') }`
}

const startStopwatch = () => {
    isStarted.value = true
    isPaused.value = false
    startTime = Date.now()
    stopwatch = setInterval(() => {
        if(!isPaused.value) {
            ms.value = elapsedTime + (Date.now() - startTime)
        }
    }, 10);
}

const pauseStopwatch = () => {
    isPaused.value = true
    elapsedTime = ms.value
    clearInterval(stopwatch)
}

const resetStopwatch = () => {
    ms.value = 0
    elapsedTime = 0
    isStarted.value = false
    isPaused.value = false

    clearInterval(stopwatch)

    lapsAmount = 0
    laps.value.length = 0
}

const newLap = () => {
    lapsAmount++
    laps.value.push({
        id: lapsAmount,
        timeElapsed: lapsAmount > 1 ? formatTime(countTimeElapsed(lapsAmount, ms.value)) : formatTime(ms.value),
        timeElapsedFromStartValue: ms.value,
        timeElapsedFromStart: formatTime(ms.value)
    })

    var audio = new Audio(stopwatchSound)
    audio.play()
}

const countTimeElapsed = (value, time) => {
    value -= 2
    return time - laps.value[value].timeElapsedFromStartValue
}
</script>

<template>
<div class="stopwatch">
    <p class="stopwatchHeader">
        {{ formatTime(ms) }} <i v-if="isPaused" class="bi bi-pause-btn-fill"></i> <span class="laps-amount d-flex d-sm-inline-flex" v-if="isStarted">{{ lapsAmount }}<span class="d-inline d-sm-none"> laps</span></span>
    </p>

    <Button v-if="!isStarted" @click="startStopwatch()"><i class="bi bi-caret-right-fill"></i> Start Stopwatch</Button>
    <Button v-if="isStarted" @click="isPaused ? startStopwatch() : pauseStopwatch()"><i  class="bi bi-pause-fill"></i> Pause Stopwatch</Button>
    <Button v-if="isStarted && isPaused" @click="resetStopwatch()"><i class="bi bi-arrow-clockwise"></i> Reset Stopwatch</Button>
    <Button v-if="isStarted" @click="newLap()"><i class="bi bi-stopwatch-fill"></i> Set a Lap</Button>

    <div class="laps">
        <Lap v-for="lap in laps" :key="lap.id" :id="lap.id" :timeElapsed="lap.timeElapsed" :timeElapsedFromStart="lap.timeElapsedFromStart" :timeElapsedFromStartValue="lap.timeElapsedFromStartValue" />
    </div>
</div>
</template>

<style scoped>
.stopwatch {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    
    margin: 1vmax 0;
    padding: 0 2vmax;
}

.stopwatchHeader {
    margin: 0;
    margin-bottom: 1vmax;

    font-size: calc(2rem + 1.5vh);
    font-weight: 900;
    text-align: center;
}

.laps-amount {
    justify-content: center;

    padding: 0 calc(0.5rem + 0.5vh);

    font-weight: 400 !important;
    text-align: center;

    border-radius: 0.25em;

    background: rgb(123, 168, 185);

    white-space: pre-wrap;
}

.stopwatchBtn {
    margin: calc(0.25rem + 0.25vh);
    padding: calc(0.25rem + 0.25vh) calc(0.5rem + 0.5vh);

    font-size: calc(0.875rem + 0.875vh);

    border: none;
    border-radius: 0.5em;
    outline: none;

    background-color: rgb(122, 41, 108);
}
.stopwatchBtn:hover {
    background-color: rgb(150, 49, 133);
}

.laps {
    width: 100%;
    margin-top: 2vmax;
}
</style>