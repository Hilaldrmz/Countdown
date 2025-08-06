<template>
    <Counter :year="year()" :month="1" :date="1" :hour="0" :minute="0" :second="0" :millisecond="0" />
</template>

<script setup>
import Counter from '../components/Counter.vue';

function year() {
    const today = new Date().getUTCFullYear()
    let newYear = today + 1
    return newYear
}

year()
</script>

<style scoped>


</style>