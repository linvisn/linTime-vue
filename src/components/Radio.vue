<script setup>
defineProps({
    name: String,
    id: String,
    value: String
})

const model = defineModel()
</script>

<template>
    <div class="radio form-check">
        <input class="radioInput form-check-input" type="radio" :name="name" :id="id" v-model="model" :value="value">
        <label class="radioLabel form-check-label" :for="id">
          {{ value }}
        </label>
    </div>
</template>

<style scoped>
.radio {
    font-size: calc(0.875rem + 0.875vh);
    font-weight: 400;
}

.radioInput {
    border: calc(0.1rem + 0.1vh) solid rgb(255, 255, 255);

    box-shadow: none;
}
.radioInput:checked {
    background-color: rgb(238, 105, 16);
}
</style>