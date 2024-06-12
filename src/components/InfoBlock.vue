<script setup>
defineProps({
    imageUrl: String,
    title: String,
    text: String,
    buttonText: String
})

const emit = defineEmits(['closedDrawer'])
</script>

<template>
    <div class="wrapper">
        <img class="image" :src="imageUrl" alt="Info image" />
        <h2 class="title">{{ title }}</h2>
        <p class="text">{{ text }}</p>
        <button @click="() => emit('closedDrawer')" class="button">{{ buttonText }}</button>
    </div>
</template>

<style scoped>
.wrapper {
    display: flex;
    flex-direction: column;
    gap: 20px;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 0 20px;
}
.image {
    width: 120px;
}
.title {
    font-size: 2rem;
    font-weight: 700;
}
.text {
    font-size: 1.125rem;
    color: #969696;
}
.button {
    border: 1px solid #f3f3f3;
    border-radius: 10px;
    padding: 5px 10px;
    outline: 0px solid rgb(190, 190, 190);
    transition: 0.2s;
    &:hover {
        outline: 1px solid rgb(190, 190, 190);
    }
}
</style>
