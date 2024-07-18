<script setup>
import { ref, watch } from 'vue'
import Button from "@/components/Button.vue"
import tabataSound from '@/assets/tabataSound.mp3'

const secondsForWork = ref(0)
const secondsForRest = ref(0)
const time = ref(0)
const rounds = ref(0)
const currentRound = ref(0)

const isStarted = ref(false)
const isPaused = ref(false)
const isWork = ref(false)

let tabata = null


watch([secondsForWork, secondsForRest, rounds], ([newSecondsForWork, newSecondsForRest, newRounds]) => {
    secondsForWork.value = Math.floor(newSecondsForWork)
    secondsForRest.value = Math.floor(newSecondsForRest)
    rounds.value = Math.floor(newRounds)
})


const startTabata = () => {
    if(secondsForWork.value >= 5 && secondsForRest.value >= 5 && secondsForWork.value <= 600 && secondsForRest.value <= 600 && rounds.value > 0) {
        isStarted.value = true
        time.value = 5
        tabata = setInterval(() => {
            if(!isPaused.value) {
                time.value -= 1

                if(time.value < 1) {
                    skipPhase()
                }
            }
        }, 1000);
    }
    else {
        alert("An amount of time for both work and rest should be greater than 5 seconds and less than 10 minutes. Also there should be at least 1 round.")
    }
}

const resetTabata = () => {
    time.value = secondsForWork.value
    currentRound.value = 0
    isStarted.value = false
    isPaused.value = false
    isWork.value = false

    clearInterval(tabata)
}

const skipPhase = () => {
    if(currentRound.value < rounds.value) {
        if(isWork.value) {
            time.value = secondsForRest.value
        }
        else {
            time.value = secondsForWork.value
            currentRound.value++
        }
        isWork.value = !isWork.value
    }
    else if (currentRound.value >= rounds.value) {
        resetTabata()
    }
    playSound()
}

const playSound = () => {
    var audio = new Audio(tabataSound)
    audio.play()
}
</script>

<template>
<div class="tabata">
    <p class="tabataHeader" :class="{ tabataHeaderWork: isWork }" v-if="isStarted">
        {{ String(Math.floor((time % 3600) / 60)).padStart(2, '0') }}:{{ String(time % 60).padStart(2, '0') }}
        <span class="label" v-if="isWork"> work </span><span class="label" v-if="!isWork"> rest </span>
        <i v-if="isPaused" class="bi bi-pause-btn-fill"></i>
        <div class="round">Round {{ currentRound }} of {{ rounds }}</div>
    </p>

    <div class="tabataInputs row">
        <span class="tabataInputItem col-lg-4 col-5">
            <div class="tabataInputLabel">
                seconds for work
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="secondsForWork = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="tabataInput" :class="{ tabataInputActive: isStarted }" type="number" placeholder="Enter seconds for work" v-model="secondsForWork" min="0" @keyup.enter="startTabata()" :disabled="isStarted">
        </span>

        <span class="tabataInputItem col-lg-3 col-4">
            <div class="tabataInputLabel">
                seconds for rest 
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="secondsForRest = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="tabataInput" :class="{ tabataInputActive: isStarted }" type="number" placeholder="Enter seconds for rest" v-model="secondsForRest" min="0" @keyup.enter="startTabata()" :disabled="isStarted">
        </span>

        <span class="tabataInputItem col-lg-2 col-3">
            <div class="tabataInputLabel">
                rounds 
                <span class="resetValue d-block d-sm-inline" v-if="!isStarted" @click="rounds = 0"><i class="bi bi-arrow-clockwise"></i></span>
            </div>
            <input class="tabataInput" :class="{ tabataInputActive: isStarted }" type="number" placeholder="Enter rounds" v-model="rounds" min="0" @keyup.enter="startTabata()" :disabled="isStarted">
        </span>
    </div>


    <Button v-if="!isStarted" @click="startTabata()"><i class="bi bi-caret-right-fill"></i> Start Tabata</Button>
    <Button v-if="isStarted" @click="isPaused = !isPaused"><i class="bi bi-pause-fill"></i> Pause Tabata</Button>
    <Button v-if="isStarted && isPaused" @click="resetTabata()"><i class="bi bi-arrow-clockwise"></i> Reset Tabata</Button>
    <Button v-if="isStarted" @click="skipPhase()"><i class="bi bi-skip-end-fill"></i> Skip Phase</Button>
</div>
</template>

<style scoped>
.tabata {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    
    padding: 1vmax 2vmax;
}

.tabataHeader {
    margin: 0;
    margin-bottom: 2vh;
    padding: 0 calc(0.75rem + 0.75vh);

    font-size: calc(2rem + 1.5vh);
    font-weight: 900;
    text-align: center;

    border-radius: 0.25em;
    
    background: green;
}
.tabataHeaderWork {
    background: red;
}

.tabataInputs {
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

.tabataInputItem {
    padding: 0;
}

.tabataInputLabel {
    font-size: calc(0.625rem + 0.875vh);

    opacity: 0.5;
}

.tabataInput {
    display: block;

    width: 100%;
    height: calc(1.5rem + 2vh);
    padding: 0.25vmax 1vmax;

    border: 0.1vmax solid rgba(255, 255, 255, 0.5);
    outline: none;

    background-color: darkslategray;
}
.tabataInputActive {
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

.label {
    font-size: calc(1.5rem + 1vh);
    font-weight: 700;

    opacity: 0.75;
}

.round {
    font-size: calc(1.75rem + 1.25vh);
    font-weight: 500;
}
</style>