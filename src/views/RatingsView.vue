<template>
    <div class="flex flex-col items-center">
        <h1 class="text-5xl text-black font-medium my-4 mb-16 text-center">Рейтинги пользователей</h1>

        <button class="border border-gray-500 p-2 rounded-lg hover:bg-gray-200 transition ">
            <p v-if="isShowingAll" @click="showOnlyMine">Показать только мои</p>
            <p v-else @click="showAllRatings">Показать все</p>
        </button>

        <table class="w-8/12">
            <thead>
            <tr>
                <th class="px-4 py-2 text-xl">Никнейм</th>
                <th class="px-4 py-2 text-xl">Результат</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="rating in ratings.data" class="border ">
                <td class="px-4 py-2 text-center">{{ rating.nickname }}</td>
                <td class="px-4 py-2 text-center">{{Math.floor(rating.result/60000)}}m : {{ Math.floor(rating.result/1000) }}.{{ rating.result % 1000}}s</td>
            </tr>
            </tbody>
        </table>
    </div>

</template>

<script setup>

import {onMounted, reactive, ref} from "vue";

const ratings = reactive({
    data: []
})
const isShowingAll = ref(true)
const fetchRatings = async () => {
    const response = await fetch('https://62f17d58b1098f1508018e86.mockapi.io/ratings');
    const data = await response.json();
    ratings.data = data;
}

const showAllRatings = async () => {
    await fetchRatings()
    isShowingAll.value = true;
}
const showOnlyMine = () => {
    ratings.data = ratings.data.filter(rating => rating.nickname === getLocalStorage('name'));
    isShowingAll.value = false;
}

const getLocalStorage = (key) => {
    return localStorage.getItem(key)
}

onMounted(async () => {
    await fetchRatings();
    console.log(ratings)
})

</script>

<style scoped>
</style>