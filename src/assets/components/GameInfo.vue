<script setup>
const props = defineProps({
    score: {
        type: Number
    }
})
</script>

<template>
    <div class="logo-and-scores flex justify-between items-center">
        <div class="logo text-7xl font-bold mb-2">
            2048
        </div>
        <div class="scores justify-self-center pb-1 px-9 bg-gray-500 text-white font-bold text-xl mr-10 rounded">
            <div class="score">
                <p>score</p>
                <p class=" text-center">{{ score }}</p>
            </div>
        </div>
        </div>
        <div class="mini-rules mb-5">
            Join the numbers and get to a <span class="font-bold">2048</span> tile!
        </div>
</template>

<style scoped>
   .logo, .mini-rules{
    color: rgb(75, 74, 74);
   }
</style>