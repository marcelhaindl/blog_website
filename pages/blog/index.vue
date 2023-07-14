<template>
  <main>
    <h1>All Blogs</h1>
    <ContentList path="/blog" :query="{
      only: ['title', 'description', 'tags', '_path', 'img'],
      where: {
        tags: {
          $contains: filter,
        },
      },
      $sensitivity: 'base',
    }">
      <!-- Default list slot -->
      <template v-slot="{ list }">
        <div class="mt-12 grid grid-cols-3 gap-x-4 gap-y-12">
          <BlogItem v-for="article in list" :to="article._path" :title="article.title" :description="article.description"
            :tags="article.tags" :img="article.img" />
        </div>
      </template>

      <!-- Not found slot to display message when no content us is found -->
      <template #not-found>
        <p>No articles found.</p>
      </template>
    </ContentList>
  </main>
</template>


<script setup>
// set meta for page
useHead({
  title: "All articles",
  meta: [{ name: "description", content: "Here's a list of all my great articles" }],
});
</script>