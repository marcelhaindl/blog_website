<!-- Previous and next blog -->

<script setup>
// Import left and right arrow icon for the buttons
import { ArrowLeftIcon, ArrowRightIcon } from "@heroicons/vue/24/solid";
// Define previous and next blog article inputs
defineProps(["prev", "next"]);
</script>

<template>
  <!-- Creates a new List box (rectangle with border around it that takes in the entire screen width) -->
  <ul class="prev-next-cont">
    <!-- Creates a new list item for previous blog -->
    <li class="link-item prev">
      <!-- If there is a previous blog, then create a new link with an arrow (left) and the previous blog title -->
      <NuxtLink v-if="prev" :to="prev._path">
        <ArrowLeftIcon class="icon stroke w-6" />
        <span> {{ prev.title }} </span>
      </NuxtLink>
    </li>
    <!-- Creates a new list item for next blog -->
    <li class="link-item next">
      <!-- If there is a next blog, then create a new link with an arrow (right) and the next blog title -->
      <NuxtLink v-if="next" :to="next._path">
        <span> {{ next.title }} </span>
        <ArrowRightIcon class="icon stroke w-6" />
      </NuxtLink>
    </li>
  </ul>
</template>

<style scoped>
/* Creates a rectangular container that contains both the links (prev and next) */
.prev-next-cont {
  @apply flex gap-4 justify-between p-4 border border-slate-200 rounded-lg;
}

/* Link Item container for Title and arrow */
.link-item a {
  @apply flex gap-2;
}
</style>