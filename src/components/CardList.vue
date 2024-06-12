<script setup>
import Card from './Card.vue'

const props = defineProps({
    items: Array,
    isFavorite: Boolean
})
const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
    <div v-auto-animate class="products--wrapper">
        <Card
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :price="item.price"
            :imageUrl="item.imageUrl"
            :isFavorite="item.isFavorite"
            :onClickFavorite="isFavorite ? null : () => emit('addToFavorite', item)"
            :onClickAdd="() => emit('addToCart', item)"
            :isAdded="item.isAdded"
        />
    </div>
</template>

<style scoped>
.products--wrapper {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    padding-top: 35px;
}
</style>
