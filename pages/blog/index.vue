<!-- List all blogs -->

<template>
  <main>
    <!-- Headline "All Blogs" -->
    <h1>All Blogs</h1>
    <!-- The Content List componennt provided by Nuxt Content is used to query all blogs from the /blogs folder inside the /content folder -->
    <ContentList path="/blog" :query="{
      // It takes the title, description, tags, path and image of the article to display its overview
      only: ['title', 'description', 'tags', '_path', 'img'],
    }">
      <!-- Creates a new template with the list variable... -->
      <!-- The list variable receives data from the upper parent, so from the Content List -->
      <template v-slot="{ list }">
        <!-- Creates a grid with 3 columns -->
        <div class="mt-12 grid grid-cols-3 gap-x-4 gap-y-12">
          <!-- For each article inside the list, create a new Blog Item -->
          <BlogItem v-for="article in list" :to="article._path" :title="article.title" :description="article.description"
            :tags="article.tags" :img="article.img" />
        </div>
      </template>

      <!-- Not found slot to display message when no content is found -->
      <template #not-found>
        <p>No articles found.</p>
      </template>
    </ContentList>
  </main>
</template>


<script setup>
// Set page meta data
useHead({
  // Set title
  title: "All articles",
  // Set description
  meta: [{ name: "description", content: "Here's a list of all my great articles" }],
});
</script>