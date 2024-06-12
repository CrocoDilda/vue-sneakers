<script setup>
import axios from 'axios'
import { onMounted, ref, inject } from 'vue'
import CardList from '../components/CardList.vue'

const favorites = ref([])
const { onClickAddPlus, addToFavorite } = inject('cartsEvents')

onMounted(async () => {
    try {
        const { data } = await axios.get(
            'https://a81fed78f2c4959d.mokky.dev/favorites?_relations=items'
        )
        data.forEach((element) => {
            favorites.value.push(element.item)
        })
        console.log(favorites)
    } catch (error) {
        console.log(error)
    }
})
</script>
<template>
    <h1>Мои закладки</h1>
    <CardList :items="favorites" :isFavorite="true" @add-to-cart="onClickAddPlus" />
</template>

<!-- `https://a81fed78f2c4959d.mokky.dev/favorites/${item.favoriteId}` -->
