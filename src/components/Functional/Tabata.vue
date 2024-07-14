<script setup>
import { ref } from 'vue'
import Button from "@/components/Button.vue"
import tabataSound from '@/assets/tabataSound.mp3'

const secondsForWork = ref(0)
const secondsForRest = ref(0)
const time = ref(0)
const rounds = ref(0)
const currentRound = ref(0)

const isStarted = ref(false)
const isWork = ref(false)

let tabata = null


const startTabata = () => {
    if(secondsForWork.value >= 5 && secondsForRest.value >= 5 && secondsForWork.value <= 600 && secondsForRest.value <= 600 && rounds.value > 0) {
        isStarted.value = true
        time.value = 5
        tabata = setInterval(() => {
            time.value -= 1

            if(time.value == 3) {
                var audio = new Audio(tabataSound)
                audio.play()
            }
            if(time.value < 1) {
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
    isWork.value = false

    clearInterval(tabata)
}
</script>

<template>
<div class="tabata">
    <p class="tabataHeader" v-if="isStarted">
        {{ String(Math.floor((time % 3600) / 60)).padStart(2, '0') }}:{{ String(time % 60).padStart(2, '0') }}
        <span v-if="isWork"> - Work -</span>
        <span v-if="!isWork"> - Rest -</span>
         round {{ currentRound }} of {{ rounds }}
    </p>

    <div class="tabataInputs row">
        <span class="tabataInputItem col-lg-3 col-4">
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
    <Button v-if="isStarted" @click="resetTabata()"><i class="bi bi-arrow-clockwise"></i> Reset Tabata</Button>
</div>
</template>

<style scoped>
.tabata {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    
    padding: 1vmax 0;
}

.tabataHeader {
    margin: 0;

    font-size: calc(2rem + 1.5vh);
    font-weight: 900;
    text-align: center;
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
</style>