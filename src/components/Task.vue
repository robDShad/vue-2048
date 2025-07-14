<script setup>
import { ref, computed } from 'vue';

const songs = ref(['First song', 'Second song', 'Third song', 'Fourth song', 'Fifth song']);
const isShuffling = ref(false)
const inputVal = ref(1);
const timer = ref(null)

const shuffleList = () => {
  return songs.value.sort(() => Math.random() - 0.5);
}
const mixingInterval = computed(() => inputVal.value * 1000);

const turnOnMixing = () => {
    clearInterval(timer.value)
    if (isShuffling.value) {
    timer.value = setInterval(() => {
        shuffleList();
    }, mixingInterval.value)}
};

const handleMixing = () => {
    isShuffling.value = !isShuffling.value;
    turnOnMixing();
}

</script>

<template>
    <ol class="songs-container flex flex-col gap-2 p-5 border-2 border-black">
        <li class="song-name " v-for="song in songs">
        {{ song }}
        </li>
    </ol>
        <input v-model="inputVal" :disabled="isShuffling" type="number" placeholder="Enter interval of mixes..." class="interval-input p-3 border-1 border-black hover:bg-gray-300 mr-5 disabled:hover:cursor-not-allowed disabled:bg-gray-300">
        <button class="mix-switch p-3 border-1 border-black hover:bg-gray-300" @click="handleMixing">
            {{ isShuffling ? 'Stop mixing' : 'Start mixing' }}
            </button> 
</template>