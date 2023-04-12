<script setup>

import {onMounted, onUpdated, reactive, ref, watchEffect} from "vue";

const stateLetters = reactive({
    letters: ['a', 'b', 'c', 'd', 'e', 'f', 'g'],
    chosenLetters: []
})

//, 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'

const elapsed = ref(0)
const startTime = ref(null)
const timer = ref(null)
const displayTime = ref('00:00:00.000');

const nickName = ref('')
const setNickname = () => {
    localStorage.setItem('name', nickName.value)
    nickName.value = ''
}

let sortedLetters = [...stateLetters.letters]
sortedLetters.sort((a, b) => a.localeCompare(b))

const getLocalStorage = (key) => {
    return localStorage.getItem(key)
}
const random = () => stateLetters.letters.sort(() => Math.random() - 0.5);

const handleUpdate = () => {
    random()
    stopStopwatch()
    elapsed.value = 0
    stateLetters.chosenLetters = []
}

const handleClickLetter = (letter) => {
    if (stateLetters.chosenLetters.includes(letter)) {
        stateLetters.chosenLetters.splice(stateLetters.chosenLetters.indexOf(letter), 1)
    } else {
        if(letter === 'a') startStopwatch()
        stateLetters.chosenLetters.push(letter)
    }

    if (stateLetters.chosenLetters.length === sortedLetters.length) {
        for (let i = 0; i < stateLetters.letters.length; i++) {
            if (stateLetters.chosenLetters[i] != sortedLetters[i]) {
                return
            }
        }
        stopStopwatch()
        fetch('https://62f17d58b1098f1508018e86.mockapi.io/ratings', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                nickname: getLocalStorage('name'),
                result: elapsed.value,
            })
        })
        elapsed.value = 0
        alert('YOU WIN!')
    }
}

const chosenLetterBg = (letter) => {
    if (sortedLetters.indexOf(letter) === stateLetters.chosenLetters.indexOf(letter)) {
        return 'bg-green-500'
    }
    return 'bg-red-500'
}


const startStopwatch = () => {
    startTime.value = Date.now() - elapsed.value;
    timer.value = setInterval(() => {
        elapsed.value = Date.now() - startTime.value;
    }, 1000);
}

const stopStopwatch = () => {
    clearInterval(timer.value);
}

watchEffect(() => {
    const seconds = Math.floor(elapsed.value / 1000) % 60;
    const minutes = Math.floor(elapsed.value / (60 * 1000)) % 60;
    displayTime.value = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
});

onMounted(() => {
    random()
})

onUpdated(() => {
    console.log(timer.value)
})

</script>

<template>
    <main class="flex flex-col items-center">
        <h1 class="text-5xl text-black font-medium my-4 text-center">От а до я</h1>

        <div v-if="!getLocalStorage('name')" class="flex gap-4">
            <input v-model="nickName" placeholder="Ведите никнейм" class="p-1 border border-gray-400 rounded-lg"/>
            <button class="p-1 border border-gray-400 rounded-lg" @click="setNickname">Сохранить</button>
        </div>
        <div v-else>
            <p class="text-xl text-black font-medium my-2 text-center">
                Привет, <span class="text-2xl text-gray-500">{{ getLocalStorage('name') }}</span>
            </p>
        </div>

        <p class="text-xl text-black font-medium my-4 text-center">Нажимайте на буквы настолько быстро, насколько
            сможете!</p>


        <p class="underline text-gray-600 mb-2 text-center cursor-pointer" @click="handleUpdate">обновить</p>


        <p class=" text-gray-600 text-center mb-4 cursor-pointer">Таймер: <span class="text-blue-500">{{
            displayTime
            }}</span>
        </p>

        <div class=" flex flex-wrap gap-4 justify-center w-8/12">
            <div v-for="(letter, index) in stateLetters.letters" :key="index" @click="handleClickLetter(letter)"
                 class="cursor-pointer hover:scale-110 transition p-14 w-20 h-20 flex items-center justify-center text-white text-5xl rounded-xl"
                 :class="stateLetters.chosenLetters.includes(letter) ? chosenLetterBg(letter) : 'bg-blue-400'"
            >
                {{ letter }}
            </div>
        </div>

    </main>
</template>
