<script setup>
defineProps({
    id: String,
    prevId: String,
    icon: String,
    windowType: String
})
defineEmits(['switchWindow'])
</script>

<template>
    <div class="window row" :id="id">
        <div class="col-md-5 col-11" :class="windowType">
            <div class="windowHeader sticky-top" :class="{ windowHeaderMoved: prevId, 'text-center': prevId, 'text-lg-start': prevId }">
                <button class="windowPrevBtn" v-if="prevId" @click="$emit('switchWindow', id, prevId)"><i class="bi bi-arrow-left-circle-fill"></i></button>
                <i class="bi" :class="icon"></i> {{ id }}
                <button class="windowExitBtn" @click="$emit('switchWindow', id)"><i class="bi bi-x-circle-fill"></i></button>
            </div>
            <div class="windowContent">
                <slot />
            </div>
        </div>
    </div>
</template>

<style scoped>
.window {
    display: flex !important;
    justify-content: center !important;
    align-items: center !important;

    position: absolute;
    top: 0;
    left: 0;

    width: 100% !important;
    height: 100% !important;
    padding: 0 !important;
    margin: 0 !important;

    font-size: calc(1rem + 1.35vh);
    font-weight: 700;

    background: rgba(30, 48, 48, 0.5);

    z-index: -1;
}
.window-active {
    z-index: 2;
}

.windowHeader {
    width: 100%;
    min-height: calc(1.5rem + 2vh);
    
    padding: 2.375vmax 3vmax;
    margin-bottom: 2vh;

    text-shadow: 0 0 1em black;

    background: linear-gradient(rgba(0, 0, 0, 0.25), transparent);
}
.windowHeaderMoved {
    padding: 2.375vmax 7vmax;
}


.settingsWindow, .aboutWindow, .customizationWindow, .toolsWindow {
    position: relative;

    height: 75%;
    padding: 0 !important;
    padding-bottom: 2vmax !important;

    border: 0.1vmax solid rgba(255, 255, 255, 0.5);
    border-radius: 0.5em;
    
    overflow-y: auto;
}
.settingsWindow {
    background: rgb(31, 31, 78);
}
.aboutWindow {
    text-align: center;

    background: rgb(78, 71, 31);
}
.customizationWindow {
    background: rgb(110, 67, 26);
}
.toolsWindow {
    background: rgb(102, 95, 30);
}


.windowExitBtn, .windowPrevBtn {
    position: absolute;
    top: 0;
    right: 0;

    width: fit-content;
    height: fit-content;
    padding: 2.125vmax 3vmax;

    font-size: calc(0.75rem + 1.75vh);

    border: 0;
    outline: 0;

    background: transparent;
    
    opacity: 0.75;
}
.windowExitBtn i, .windowPrevBtn i {
    color: rgb(255, 255, 255) !important;
}
.windowExitBtn:hover, .windowPrevBtn:hover {
    opacity: 1;
}

.windowPrevBtn {
    left: 0;
}


.windowContent {
    padding: 0 3vmax;
}
</style>