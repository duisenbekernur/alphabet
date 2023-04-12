<script setup>
import Navbar from "@/components/Navbar.vue";
</script>

<template>
    <div>
        <Navbar/>
        <main class="app">
            <RouterView/>
        </main>
    </div>
</template>

<style scoped>
.app {
    width: 1440px;
    margin: 0 auto;
}
</style>
