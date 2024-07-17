<script setup>
import { ref, watch } from 'vue'
import Button from "@/components/Button.vue"
import timerSound from '@/assets/timerSound.mp3'

const seconds = ref(0)
const minutes = ref(0)
const hours = ref(0)
const time = ref(0)

const isStarted = ref(false)
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
            <div class="timerInputLabel">
                hours 
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="hours = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="timerInput" :class="{ timerInputActive: isStarted }" type="number" placeholder="Enter hours" v-model="hours" min="0" @keyup.enter="startTimer()" :disabled="isStarted">
        </span>

        <span class="timerInputItem col-3">
            <div class="timerInputLabel">
                minutes 
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="minutes = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="timerInput" :class="{ timerInputActive: isStarted }" type="number" placeholder="Enter minutes" v-model="minutes" min="0" @keyup.enter="startTimer()" :disabled="isStarted">
        </span>

        <span class="timerInputItem col-3">
            <div class="timerInputLabel">
                seconds 
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="seconds = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="timerInput" :class="{ timerInputActive: isStarted }" type="number" placeholder="Enter seconds" v-model="seconds" min="0" @keyup.enter="startTimer()" :disabled="isStarted">
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

.timerInputLabel {
    font-size: calc(0.625rem + 0.875vh);

    opacity: 0.5;
}

.timerInput {
    display: block;

    width: 100%;
    height: calc(1.5rem + 2vh);
    padding: 0.25vmax 1vmax;

    border: 0.1vmax solid rgba(255, 255, 255, 0.5);
    outline: none;

    background-color: darkslategray;
}
.timerInputActive {
    background-color: rgb(128, 123, 65);

    opacity: 0.75;
}

.resetValue {
    margin: 0.5vmax 0.25vmax 1.25vmax;
    padding: 0.25vmax 1vmax;

    background-color: rgb(41, 122, 95);
}
.resetValue:hover {
    background-color: rgb(37, 163, 121);
}
</style>