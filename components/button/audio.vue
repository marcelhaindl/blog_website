<template>
    <audio class="hidden" :src="`/sounds/${sound}.wav`" controls ref="audioTag" @ended="ended()"></audio>
    <button class="transition h-64 w-full rounded bg-neutral-50 hover:bg-neutral-100 border border-neutral-200 shadow-md hover:shadow-sm" @click="playSound()" :class="{ playing: isPlaying }"><h3 class="text-neutral-600">{{ soundText }}</h3></button>
</template>


<script setup>
import { ref } from "vue";

// Define sound input
const props = defineProps(["sound"]);

// Change sountext from "clap" to "Clap"
const soundText = computed(() => {
    return props.sound.charAt(0).toUpperCase() + props.sound.slice(1)
})

// Reference variables
const audioTag = ref();
const isPlaying = ref(false);

// Plays the sound
const playSound = () => {
    isPlaying.value = true;
    audioTag.value.play();
}

// Sets the isPlaying value to false if sound finished
const ended = () => {
    isPlaying.value = false;
}
</script>