<script setup>
import { ref, watch } from 'vue'
import Button from "@/components/Button.vue"
import timerSound from '@/assets/timerSound.mp3'

const seconds = ref(0)
const minutes = ref(0)
const hours = ref(0)
const time = ref(0)

const isStarted = defineModel('isStarted')
const isPaused = ref(false)

let timer = null


watch([seconds, minutes, hours], ([newSeconds, newMinutes, newHours]) => {
    seconds.value = Math.floor(newSeconds)
    minutes.value = Math.floor(newMinutes)
    hours.value = Math.floor(newHours)
    time.value = (newSeconds + newMinutes * 60 + newHours * 3600)
})


const startTimer = () => {
    if(time.value > 0 && time.value <= 120 * 3600) {
        isStarted.value = true
        
        timer = setInterval(() => {
            if(!isPaused.value) {
                time.value -= 1

                if(time.value < 1) {
                    resetTimer()
                    var audio = new Audio(timerSound)
                    audio.play()
                }
            }
        }, 1000);
    }
    else {
        alert("An amount of time should be greater than 1 second and less than 120 hours.")
    }
}

const resetTimer = () => {
    time.value = (seconds.value + minutes.value * 60 + hours.value * 3600)
    isStarted.value = false
    isPaused.value = false

    clearInterval(timer)
}
</script>

<template>
<div class="timer">
    <p class="timerHeader"> 
        <span v-if="Math.floor(time / 3600) > 0">{{ String(Math.floor(time / 3600)).padStart(2, '0') }}:</span>{{ String(Math.floor((time % 3600) / 60)).padStart(2, '0') }}:{{ String(time % 60).padStart(2, '0') }}
        <i v-if="isPaused" class="bi bi-pause-btn-fill"></i>
    </p>


    <div class="timerInputs row">
        <span class="timerInputItem col-3">
            <div>
                <span class="timerInputLabel">hours </span>
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="hours = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="timerInput first" :class="{ timerInputActive: isStarted }" type="number" placeholder="Enter hours" v-model="hours" min="0" @keyup.enter="startTimer()" :disabled="isStarted">
        </span>

        <span class="timerInputItem col-3">
            <div>
                <span class="timerInputLabel">minutes </span>
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="minutes = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="timerInput second" :class="{ timerInputActive: isStarted }" type="number" placeholder="Enter minutes" v-model="minutes" min="0" @keyup.enter="startTimer()" :disabled="isStarted">
        </span>

        <span class="timerInputItem col-3">
            <div>
                <span class="timerInputLabel">seconds </span>
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="seconds = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="timerInput third" :class="{ timerInputActive: isStarted }" type="number" placeholder="Enter seconds" v-model="seconds" min="0" @keyup.enter="startTimer()" :disabled="isStarted">
        </span>
    </div>


    <Button v-if="!isStarted" @click="startTimer"><i class="bi bi-caret-right-fill"></i> Start Timer</Button>
    <Button v-if="isStarted" @click="resetTimer"><i class="bi bi-arrow-clockwise"></i> Reset Timer</Button>
    <Button v-if="isStarted" @click="isPaused = !isPaused"><i class="bi bi-pause-fill"></i> Pause Timer</Button>
</div>
</template>

<style scoped>
.timer {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    
    padding: 1vmax 0;
}

.timerHeader {
    margin: 0;

    font-size: calc(2rem + 1.5vh);
    font-weight: 900;
}

.timerInputs {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: flex-end;
    justify-content: center;

    margin: 0;
    margin-top: 0.5vmax;
    margin-bottom: 2vmax;

    text-align: center;
}

.timerInputItem {
    padding: 0;
}

.timerInputItem div {
    font-size: calc(0.625rem + 0.875vh);
}

.timerInputLabel {
    opacity: 0.5;
}


.timerInput {
    display: block;

    width: 100%;
    height: calc(1.5rem + 2vh);
    padding: 0.25vmax 1vmax;

    border: 0.1em solid rgba(255, 255, 255, 0.5);
    outline: none;

    background-color: darkslategray;
}
.timerInput:is(.second) {
    border-right: none;
    border-left: none;
}

.midnight-fireplace .timerInput {
    background-color: rgb(64, 47, 79);
}
.mocha .timerInput {
    border-color: rgb(var(--ctp-mocha-lavender-rgb));

    background-color: rgb(var(--ctp-mocha-overlay0-rgb));
}
.mocha .timerInput:is(.first) {
    border-top-left-radius: 0.5em;
    border-bottom-left-radius: 0.5em;
}
.mocha .timerInput:is(.third) {
    border-top-right-radius: 0.5em;
    border-bottom-right-radius: 0.5em;
}

.timerInputActive {
    background-color: rgb(128, 123, 65);

    opacity: 0.75;
}
.midnight-fireplace .timerInputActive {
    background-color: rgb(77, 79, 47);
}
.mocha .timerInputActive {
    border-color: rgb(var(--ctp-mocha-yellow-rgb));
}


.resetValue {
    margin: 0.5vmax 0.25vmax 1.25vmax;
    padding: 0.25vmax 1vmax;

    background-color: rgb(41, 122, 95);
    
    opacity: 0.75;
}
.resetValue:hover {
    background-color: rgb(37, 163, 121);

    opacity: 1;
}

.midnight-fireplace .resetValue {
    background-color: rgb(165, 18, 18);
}
.midnight-fireplace .resetValue:hover {
    background-color: rgb(204, 33, 33);
}

.mocha .resetValue {
    background-color: rgb(var(--ctp-mocha-sky-rgb));
}
.mocha .resetValue:hover {
    background-color: rgb(var(--ctp-mocha-blue-rgb));
}
</style>