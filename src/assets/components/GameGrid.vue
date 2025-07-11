<script setup>
    const props = defineProps({
        gameGrid: {
            type: Array,
        },
    })
</script>

<template>
    <div class="game-board flex flex-col gap-3 justify-center items-center py-3 mb-3">
        <div class="grid-row flex gap-3" v-for="row in gameGrid">
            <div :class="`${cell.color()} w-29 h-29 flex justify-center items-center text-gray-500 text-5xl font-bold transition-colors ease-in-out duration-50`" v-for="cell in row">{{ cell.value }}</div>
        </div>
    </div>
</template>

<style scoped>
   .game-board {
    background-color: rgb(75, 74, 74);
   }
</style>