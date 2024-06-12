<script setup>
import CardList from '../components/CardList.vue'
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted } from 'vue'

const { cart, addToCart, removeFromCart } = inject('cart')
const { onClickAddPlus, addToFavorite } = inject('cartsEvents')

const items = ref([])

const filters = reactive({
    sortBy: 'title',
    searchQuery: ''
})

const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get(`https://a81fed78f2c4959d.mokky.dev/favorites`)
        items.value = items.value.map((item) => {
            const favorite = favorites.find((favorite) => favorite.item_id === item.id)
            if (!favorite) {
                return item
            }

            return {
                ...item,
                isFavorite: true,
                favoriteId: favorite.id
            }
        })
    } catch (error) {
        console.log(error)
    }
}
const fetchItem = async () => {
    try {
        const parameters = {
            sortBy: filters.sortBy
        }

        if (filters.searchQuery) {
            parameters.title = `*${filters.searchQuery}*`
        }
        const { data } = await axios.get(`https://a81fed78f2c4959d.mokky.dev/items`, {
            params: parameters
        })
        items.value = data.map((obj) => ({
            ...obj,
            isFavorite: false,
            favoriteId: null,
            isAdded: false
        }))
    } catch (error) {
        console.log(error)
    }
}
onMounted(async () => {
    const localCart = localStorage.getItem('cart')
    cart.value = localCart ? JSON.parse(localCart) : []

    await fetchItem()
    await fetchFavorites()
})

watch(cart, () => {
    items.value = items.value.map((item) => ({
        ...item,
        isAdded: false
    }))
})
watch(filters, fetchItem)
</script>

<template>
    <div class="top--wrapper">
        <h2 class="text-3xl font-bold">Все кроссовки</h2>
        <select @change="onChangeSelect" class="selects">
            <option value="title">По названию</option>
            <option value="price">Сначала дешёвые</option>
            <option value="-price">Сначала дорогие</option>
        </select>
        <label class="label" for="search">
            <img src="/search.svg" alt="search" />
            <input
                @input="onChangeSearchInput"
                type="text"
                id="search"
                placeholder="Поиск..."
                class="input"
            />
        </label>
    </div>
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>

<style scoped>
.top--wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 0;
}

.selects {
    border: 1px solid #f3f3f3;
    border-radius: 10px;
    padding: 5px 10px;
    border-radius: 10px;
    outline: 0px solid rgb(190, 190, 190);
    transition: 0.2s;
    &:focus {
        outline: 1px solid rgb(190, 190, 190);
    }
}

.label {
    position: relative;
}

.label img {
    position: absolute;
    left: 10px;
    top: 50%;
    transform: translateY(-50%);
}

.input {
    display: flex;
    align-items: center;
    gap: 12px;
    border: 1px solid #f3f3f3;
    border-radius: 10px;
    padding: 5px 10px 5px 35px;
    outline: 0px solid #bebebe;
    font-size: 1.125rem;
    transition: 0.2s;
    &:focus {
        outline: 1px solid #bebebe;
    }
}
</style>
