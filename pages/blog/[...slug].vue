<!-- ./pages/blog/[…slug.vue] -->

<script setup>
const { path } = useRoute();
const { data } = await useAsyncData(`content-${path}`, async () => {
    // fetch document where the document path matches with the cuurent route
    let article = queryContent().where({ _path: path }).findOne();
    // get the surround information,
    // which is an array of documeents that come before and after the current document
    let surround = queryContent().only(["_path", "title", "description"]).sort({ date: 1 }).findSurround(path);

    return {
        article: await article,
        surround: await surround,
    };
});

// destrucure `prev` and `next` value from data
const [prev, next] = data.value.surround;
console.log({ data, prev, next });

// set the meta
useHead({
    title: data.value.article.title,
    meta: [
        { name: "description", content: data.value.article.description },
        {
            hid: "og:image",
            property: "og:image",
            content: `${data.value.article.img}`,
        },
    ],
});
</script>
<template>
    <main>
        <header v-if="data.article">
            <div class="h-96 mb-12 w-full">
                <img :src=data.article.img :alt=data.article.title class="rounded h-full w-full object-cover" />
            </div>
        </header>
        <div class="">
                <h1>{{ data.article.title }}</h1>
                <p class="subtext">{{ data.article.description }}</p>
                <ul class="article-tags">
                    <li class="tag" v-for="(tag, n) in data.article.tags" :key="n">{{ tag }}</li>
                </ul>
            </div>
        <hr />
        <section>
            <aside>
                <!-- Toc Component -->
                <Toc :links="data.article.body.toc.links" />
            </aside>
            <article>
                <!-- render document coming from query -->
                <ContentRenderer :value="data.article">
                    <template #empty>
                        <p>No content found.</p>
                    </template>
                </ContentRenderer>
            </article>
        </section>

        <!-- PrevNext Component -->
        <PrevNext :prev="prev" :next="next" class="mt-24" />
    </main>
</template>

<style scoped>
section {
    @apply grid grid-cols-8;
}

aside .toc {
    @apply sticky top-32;
}

aside {
    @apply col-span-full md:col-span-2 row-start-1 w-full pt-14;
}

article {
    @apply col-span-full md:col-span-6 md:col-start-1 md:row-start-1 prose w-full;
}

.article-tags {
    @apply mt-4
}
</style>