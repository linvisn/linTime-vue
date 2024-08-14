<template>
<button>
    <slot />
</button>
</template>

<style scoped>
button {
    margin: calc(0.25rem + 0.25vh);
    padding: calc(0.25rem + 0.25vh) calc(0.5rem + 0.5vh);

    font-size: calc(0.875rem + 0.875vh);

    border: none;
    border-radius: 0.5em;
    outline: none;

    background-color: rgb(122, 41, 108);
}
button:hover {
    background-color: rgb(150, 49, 133);
}
.midnight-fireplace button {
    background: rgb(175, 63, 19);
}
.midnight-fireplace button:hover {
    background: rgb(201, 85, 40);
}
</style>