<script setup>
const emit = defineEmits(['openDrawer'])

defineProps({
    totalPrice: Number
})
</script>

<template>
    <header class="flex justify-between border-b border-slate-300 px-10 py-8">
        <router-link to="/">
            <div class="flex items-center gap-4">
                <img src="/logo.png" alt="Logo" class="w-10" />
                <div>
                    <h2 class="text-xl font-bold uppercase">Vue sneakers</h2>
                    <p class="text-slate-400">Магазин лучших кроссовок</p>
                </div>
            </div>
        </router-link>
        <ul class="flex items-center gap-10">
            <li class="flex items-center gap-2 text-gray-500 cursor-pointer hover:text-black">
                <button @click="() => emit('openDrawer')" class="button">
                    <img src="/cart.svg" alt="cart" />
                    <b>{{ totalPrice }} rub</b>
                </button>
            </li>
            <router-link to="/favorites">
                <li class="flex items-center gap-2 text-gray-500 cursor-pointer hover:text-black">
                    <button class="button">
                        <img src="/heart.svg" alt="cart" />
                        <span>Закладки</span>
                    </button>
                </li>
            </router-link>
            <li class="flex items-center gap-2 text-gray-500 cursor-pointer hover:text-black">
                <button class="button">
                    <img src="/profile.svg" alt="cart" />
                    <span>Профиль</span>
                </button>
            </li>
        </ul>
    </header>
</template>

<style scoped>
.button {
    display: flex;
    align-items: center;
    gap: 10px;
}
</style>
