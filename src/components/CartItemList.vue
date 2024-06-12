<script setup>
import { inject } from 'vue'
import CartList from './CartItem.vue'

const { cart, removeFromCart } = inject('cart')
</script>

<template>
    <div v-auto-animate class="wrapper">
        <CartList
            v-for="item in cart"
            :key="item.id"
            :title="item.title"
            :price="item.price"
            :imageUrl="item.imageUrl"
            @on-click-remove="() => removeFromCart(item)"
        />
    </div>
</template>

<style scoped>
.wrapper {
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding-right: 20px;
}
</style>
