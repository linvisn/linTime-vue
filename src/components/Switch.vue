<script setup>
defineProps({
    text: String
})

const model = defineModel()
</script>

<template>
    <li class="switch form-check form-switch">
        <input class="switchInput form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault" v-model="model">
        <label class="switchLabel form-check-label" for="flexSwitchCheckDefault">{{ text }}</label>
    </li>
</template>

<style scoped>
.switch {
    display: flex;
    align-items: center !important;

    margin: 1vh 0;

    word-break: break-all;
}

.switchInput {
    width: 2.25em;
    height: 1.25em;
    margin-top: 0;

    border: calc(0.1rem + 0.1vh) solid rgb(255, 255, 255);

    box-shadow: none;
}
.switchInput:checked {
    background-color: rgb(16, 238, 127);
}

.switchLabel {
    margin-left: calc(1rem + 1vh);

    font-size: calc(0.875rem + 0.875vh);
    font-weight: 500;
}
</style>