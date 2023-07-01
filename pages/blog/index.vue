<!-- ./pages/blog/index.vue -->

<script setup>
// set meta for page
useHead({
  title: "All articles",
  meta: [{ name: "description", content: "Here's a list of all my great articles" }],
});
</script>
<template>
  <main>
    <header>
      <div>
        <h1>All articles</h1>
        <p class="font-medium text-lg">Here's a list of all my great articles</p>
      </div>
    </header>
    <section>
      <!-- Render list of all articles in ./content/blog using `path` -->
      <!-- Provide only defined fieldsin the `:query` prop -->
      <ContentList
        path="/blog"
        :query="{
          only: ['title', 'description', 'tags', '_path', 'img'],
          where: {
            tags: {
              $contains: filter,
            },
          },
          $sensitivity: 'base',
        }"
      >
        <!-- Default list slot -->
        <template v-slot="{ list }">
          <ul>
            <li v-for="article in list" :key="article._path">
              <NuxtLink :to="article._path">
                <div>
                  <div class="w-32 shrink-0">
                    <img :src=article.img :alt="article.title" class="rounded-lg" />
                  </div>
                  <header>
                    <h2>{{ article.title }}</h2>
                    <p>{{ article.description }}</p>
                    <ul class="article-tags">
                      <li class="tag py-0.5" v-for="(tag, n) in article.tags" :key="n">{{ tag }}</li>
                    </ul>
                  </header>
                </div>
              </NuxtLink>
            </li>
          </ul>
        </template>

        <!-- Not found slot to display message when no content us is found -->
        <template #not-found>
          <p>No articles found.</p>
        </template>
      </ContentList>
    </section>
  </main>
</template>
