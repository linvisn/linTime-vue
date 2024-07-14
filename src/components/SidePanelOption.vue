<script setup>
defineProps({
    title: String,
    icon: String,
    active: Boolean,
    MP: String,
    window: String
})
defineEmits(['switchMPState', 'switchWindow'])
</script>

<template>
    <li :class="{ active: active }" @click="$emit('switchMPState', $event, MP), $emit('switchWindow', window)">
        <div>
            <i class="bi" :class="icon"></i> 
            <span class="d-none d-md-inline">{{ title }}</span>
        </div>
    </li>
</template>

<style scoped>
li {
    display: flex;
    justify-content: center;

    margin-bottom: 1vh;
    padding: 0.25vmax 0.875vmax;

    font-size: calc(0.75rem + 0.875vh) !important;
    font-weight: 500;

    border-radius: 1em;

    background-color: rgb(50, 73, 73);
}
li:hover {
    background-color: rgb(77, 95, 95);
}
.active {
    background-color: rgb(79, 116, 116);
}

li span {
    margin-left: 1vmin;
}
</style>