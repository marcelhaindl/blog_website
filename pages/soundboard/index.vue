<template>
    <main>
        <h1>Soundboard</h1>
        <div class="grid grid-cols-3 gap-4 mt-12">
            <ButtonAudio v-for="sound in sounds" :sound="sound"/>
        </div>
    </main>
</template>
  
<script setup>
const sounds = ["clap", "hihat", "kick", "perc", "snare", "tom"];
</script>
  