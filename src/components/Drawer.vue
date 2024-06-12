<script setup>
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'
const emit = defineEmits(['createOrder', 'closedDrawer'])

const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
    buttonDisabled: Boolean
})
</script>

<template>
    <div class="background"></div>
    <div class="drawer">
        <div class="title">
            <button @click="() => emit('closedDrawer')" class="title--button">
                <svg
                    width="16"
                    height="14"
                    viewBox="0 0 16 14"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M1 7H14.7143"
                        stroke="black"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                    />
                    <path
                        d="M8.71436 1L14.7144 7L8.71436 13"
                        stroke="black"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                    />
                </svg>
            </button>
            <h2>Корзина</h2>
        </div>
        <CartItemList v-if="totalPrice" />
        <InfoBlock
            @closed-drawer="closedDrawer"
            v-if="!totalPrice"
            title="Корзина пуста"
            text="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
            buttonText="Вернуться назад"
            imageUrl="/package-icon.png"
        />
        <div v-if="totalPrice" class="wrapper--buy">
            <div class="wrapper--money">
                <p>Итого:</p>
                <div class="dashed"></div>
                <b>{{ totalPrice }} ₽</b>
            </div>
            <div class="wrapper--money">
                <p>Налог 5%:</p>
                <div class="dashed"></div>
                <b>{{ vatPrice }} ₽</b>
            </div>
            <button
                :disabled="buttonDisabled"
                @click="() => emit('createOrder')"
                class="button--buy"
            >
                Оформить заказ
            </button>
        </div>
    </div>
</template>

<style scoped>
.background {
    height: 100vh;
    width: 100vw;
    background-color: rgba(0, 0, 0, 0.5);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
}

.drawer {
    position: fixed;
    top: 0;
    right: 0;
    display: grid;
    grid-template-rows: auto 1fr auto;
    height: 100vh;
    width: 400px;
    background-color: white;
    padding: 25px;
    padding-right: 5px;
    z-index: 101;
}

.title {
    display: flex;
    align-items: center;
    gap: 15px;
    margin-bottom: 25px;
}

.title--button {
    display: flex;
    justify-content: center;
    align-items: center;
    transform: rotate(180deg);
    opacity: 0.5;
    transition: 0.2s;
    width: 35px;
    height: 35px;
    &:hover {
        opacity: 1;
        transform: translateX(-5px) rotate(180deg);
    }
}

h2 {
    font-size: 1.75rem;
    font-weight: 700;
}

.wrapper--buy {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-top: 20px;
    padding-right: 20px;
}

.wrapper--money {
    display: grid;
    grid-auto-flow: column;
    grid-template-columns: auto 1fr auto;
    gap: 10px;
}

.dashed {
    border-bottom: 1px dashed rgb(190, 190, 190);
    margin-bottom: 5px;
}

.button--buy {
    background-color: #80e049;
    padding: 15px;
    border-radius: 15px;
    margin-top: 6px;
    color: white;
    font-weight: 700;
    font-size: 1.25rem;
    transition: 0.2s;
    &:hover {
        transform: translateY(-5px);
        background-color: #66b13a;
    }
    &:active {
        background-color: #487c29;
    }
    &:disabled {
        background-color: #c9c9c9;
        pointer-events: none;
    }
}
</style>
