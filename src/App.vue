<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import HeaderBlock from './components/HeaderBlock.vue'
import Drawer from './components/Drawer.vue'

const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => totalPrice.value * 0.05)

const cartIsEmpty = computed(() => cart.value.length === 0)

const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)

function closedDrawer() {
    drawerOpen.value = false
}

function openDrawer() {
    drawerOpen.value = true
}

const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
}

const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
}

const createOrder = async () => {
    try {
        isCreatingOrder.value = true
        const { data } = await axios.post('https://a81fed78f2c4959d.mokky.dev/orders', {
            items: cart.value,
            totalPrice: totalPrice.value
        })
        cart.value = []
        return data
    } catch (error) {
        console.log(error)
    } finally {
        isCreatingOrder.value = false
    }
}

watch(
    cart,
    () => {
        localStorage.setItem('cart', JSON.stringify(cart.value))
    },
    { deep: true }
)

const onClickAddPlus = (item) => {
    if (!item.isAdded) {
        addToCart(item)
    } else {
        removeFromCart(item)
    }
}
async function addToFavorite(item) {
    try {
        console.log(item)
        if (!item.isFavorite) {
            const obj = {
                item_id: item.id
            }
            item.isFavorite = true
            const { data } = await axios.post(`https://a81fed78f2c4959d.mokky.dev/favorites`, obj)
            item.favoriteId = data.id
            console.log(data.id)
        } else {
            item.isFavorite = false
            await axios.delete(`https://a81fed78f2c4959d.mokky.dev/favorites/${item.favoriteId}`)

            item.favoriteId = null
        }
    } catch (error) {
        console.log(error)
    }
}

provide('cartsEvents', {
    onClickAddPlus,
    addToFavorite,
    removeFromCart
})

provide('cart', {
    cart,
    closedDrawer,
    openDrawer,
    addToCart,
    removeFromCart
})
</script>

<template>
    <Drawer
        @closed-drawer="closedDrawer"
        v-if="drawerOpen"
        :totalPrice="totalPrice"
        :vatPrice="vatPrice"
        @createOrder="createOrder"
        :buttonDisabled="isCreatingOrder"
    />
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 p-10">
        <HeaderBlock :total-price="totalPrice" @open-drawer="openDrawer" />
        <router-view> </router-view>
    </div>
</template>

<style scoped>
html {
    font-size: 16px;
}
</style>
