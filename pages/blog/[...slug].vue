<!-- Queries the document data and renders the document -->

<script setup>
// Get the current path
const { path } = useRoute();
// Fetch the document that is located inside the content folder of the current path
// So, /blog/test will look at /content/blog/test
const { data } = await useAsyncData(`content-${path}`, async () => {
    // Fetch the document where the path equals the path in the URL
    let article = queryContent().where({ _path: path }).findOne();
    // Get the surround data, so the previous and next article
    // From the previous and next article we only need the path, the title and their description
    let surround = queryContent().only(["_path", "title", "description"]).sort({ date: 1 }).findSurround(path);

    // Return both article and surround
    return {
        article: await article,
        surround: await surround,
    };
});

// For using the previous and next article, we need to destructure them from the surround value
const [prev, next] = data.value.surround;

// Set the page meta data
useHead({
    // Set the title
    title: data.value.article.title,
    meta: [
        // Set description
        { name: "description", content: data.value.article.description },
        {
            // Sets the hidden identifier for the Open Graph (OG) image
            hid: "og:image",
            // Identifies that is is an Open Graph (OG) image
            property: "og:image",
            // Specifies the URL of the image inside the article
            content: `${data.value.article.img}`,
        },
    ],
});
</script>

<template>
    <main>
        <!-- If there is an article, then print the image -->
        <header v-if="data.article">
            <div class="h-96 mb-12 w-full">
                <img :src=data.article.img :alt=data.article.title class="rounded h-full w-full object-cover" />
            </div>
        </header>
        <!-- Headline -->
        <div>
            <!-- Headline -->
            <h1>{{ data.article.title }}</h1>
            <!-- Description -->
            <p class="subtext">{{ data.article.description }}</p>
            <!-- For each tag, create a new list item with the "tag" class -->
            <ul class="article-tags">
                <li class="tag" v-for="(tag, n) in data.article.tags" :key="n">{{ tag }}</li>
            </ul>
        </div>
        <!-- Seperator -->
        <hr />
        <!-- Creates a section which contains the Table of Contents and the article itself -->
        <section>
            <!-- Table of Contents -->
            <aside>
                <!-- Toc Component, for displaying the table of contents with all its links -->
                <Toc :links="data.article.body.toc.links" />
            </aside>
            <!-- Article -->
            <article>
                <!-- ContentRenderer is used to render the document with the data.article value -->
                <ContentRenderer :value="data.article">
                    <!-- If no content is found, then print "No content found." -->
                    <template #empty>
                        <p>No content found.</p>
                    </template>
                </ContentRenderer>
            </article>
        </section>

        <!-- Previous and Next component to display previous and next article -->
        <PrevNext :prev="prev" :next="next" class="mt-24" />
    </main>
</template>

<style scoped>
/* The section is used to seperate article and table of contents using 8 columns */
section {
    @apply grid grid-cols-8;
}

/* The Table of Contents is styles as sticky to make it stick to the top of the screen even when scrolling */
aside .toc {
    @apply sticky top-32;
}

/* The table of contents takes 2 column of the total 8 */
aside {
    @apply col-span-full md:col-span-2 row-start-1 w-full pt-14;
}

/* The article itself takes 6 columns of the total 8 */
article {
    @apply col-span-full md:col-span-6 md:col-start-1 md:row-start-1 prose w-full;
}

/* The article tags need to have a top margin of 4 */
.article-tags {
    @apply mt-4
}
</style>