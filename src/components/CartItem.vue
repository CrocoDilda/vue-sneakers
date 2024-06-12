<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
    imageUrl: String,
    id: Number,
    title: String,
    price: Number
})
</script>

<template>
    <div class="item">
        <img class="item--image" :src="imageUrl" alt="sneaker" />
        <div class="item--description">
            <h3>Мужские кроссовки Nike Blazer Mid Suede</h3>
            <div class="wrapper">
                <b>1324 руб.</b>
                <button @click="emit('onClickRemove')" class="item--button">
                    <img src="/close.svg" alt="added" />
                </button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.item {
    display: flex;
    gap: 20px;
    border: 1px solid #f3f3f3;
    border-radius: 20px;
    padding: 20px;
}

.wrapper {
    display: flex;
    justify-content: space-between;
}

.item--image {
    height: 60px;
}
</style>
