<script setup>
import { ref } from 'vue';

const songs = ref(['First song', 'Second song', 'Third song', 'Fourth song', 'Fifth song']);

</script>

<template>
    <div class="songs-container">
        <div class="song-name flex flex-col gap-2 p-5 border-2 border-black" v-for="(song, index) in songs">
            {{ song }}
        </div>
        <input type="number" placeholder="Enter interval of mixes..." class="interval-input p-3 border-1 border-black hover:bg-gray-300">
        <button class="mix-switch border-1 border-black hover:bg-gray-300">Enable/disable mixing</button>
    </div>
</template>