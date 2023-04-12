<template>
    <div class="bg-gray-900">

        <nav class="w-[1440px] m-auto p-0  border-gray-200 bg-gray-900">
            <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
                <router-link to="/home" class="flex items-center">
                    <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">Word game</span>
                </router-link>
                <button data-collapse-toggle="navbar-default" type="button"
                        class="inline-flex items-center p-2 ml-3 text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
                        aria-controls="navbar-default" aria-expanded="false">
                    <span class="sr-only">Open main menu</span>
                    <svg class="w-6 h-6" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20"
                         xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd"
                              d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
                              clip-rule="evenodd"></path>
                    </svg>
                </button>
                <div class="hidden w-full md:block md:w-auto" id="navbar-default">
                    <ul class="font-medium flex flex-col p-4 md:p-0 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">
                        <li v-for="(link, index) in links" :key="index">
                            <router-link :to="link.toLowerCase()" @click="activeItem = index"
                                         class="block py-2 pl-3 pr-4  bg-blue-700 rounded text-white  md:bg-transparent  md:p-0 "
                                         :class="{'underline': index === activeItem}"
                            >{{ link }}
                            </router-link>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </div>
</template>

<script setup>
import {reactive, ref} from "vue";

const links = reactive(['Home', 'Ratings'])
const activeItem = ref(0)

</script>

<style scoped>

</style>