<template>
    <NuxtLink :to="to" class="transition w-full rounded bg-neutral-50 hover:bg-neutral-100 border border-neutral-200 shadow-md hover:shadow-sm p-4 flex flex-col gap-y-3">
        <img :src="img" :alt="title" class="h-64 w-full object-cover rounded">
        <div class="flex flex-col justify-center">
            <h2>{{ title }}</h2>
            <p class="subtext">{{ description }}</p>
            <ul class="article-tags mt-4">
                    <li class="tag" v-for="(tag, n) in tags" :key="n">{{ tag }}</li>
                </ul>
        </div>
    </NuxtLink>
</template>

<script setup>

const props = defineProps(["to", "img", "title", "description", "tags"])

</script>