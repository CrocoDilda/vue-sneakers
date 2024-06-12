<script setup>
defineProps({
    id: Number,
    imageUrl: String,
    title: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickFavorite: Function,
    onClickAdd: Function
})
</script>

<template>
    <div class="card">
        <button v-if="onClickFavorite" @click="onClickFavorite" class="button--favorite">
            <img :src="isFavorite ? '/like-2.svg' : '/like-1.svg'" alt="added" />
        </button>
        <div class="image--wrapper">
            <img class="image--main" :src="imageUrl" alt="sneaker" />
        </div>
        <p class="title">{{ title }}</p>
        <div class="description">
            <div class="">
                <span class="price">Цена: </span>
                <b>{{ price }} Руб</b>
            </div>
            <button @click="onClickAdd" class="button--plus">
                <img :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="added" />
            </button>
        </div>
    </div>
</template>

<style scoped>
.card {
    display: grid;
    grid-auto-flow: row;
    grid-template-rows: 70% auto auto;
    justify-content: center;
    gap: 10px;
    position: relative;
    outline: 1px solid rgb(236, 236, 236);
    border-radius: 35px;
    padding: 15px;
    cursor: pointer;
    background-color: #ffffff;
    transition: 0.2s;
    &:hover {
        transform: translateY(-10px);
        box-shadow: 0 15px 15px rgb(226, 226, 226);
    }
}

.image--wrapper {
    width: 100%;
}

.image--main {
    width: 100%;
    height: 100%;
}
.description {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    position: relative;
}

.price {
    color: #94a3b8;
}

button {
    position: absolute;
}

.button--favorite {
    top: 35px;
    left: 35px;
}
.button--plus {
    top: 0;
    right: 0;
}
</style>
