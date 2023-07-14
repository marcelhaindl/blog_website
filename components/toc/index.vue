<!-- Table of contents -->

<script setup>
// Define input links
defineProps(["links"]);

// Helper function to flatten all links into one level (no hierarchy)
// The hierarchy is later done with the depth value inside the links
const flattenLinks = (links) => {
  // _links variable that maps through all the links
  let _links = links
    .map((link) => {
      // Saves each link into a new array
      let _link = [link];
      // If the link has any children...
      if (link.children) {
        // ... Then recursively call the function again to flatten the children
        let flattened = flattenLinks(link.children);
        // Then add the flattened links to the previously created link array
        _link = [link, ...flattened];
      }
      // And return the _link variable so the the upper one can use it
      return _link;
    })
    // And set the depth to 1
    .flat(1);

  // Finally return the _links variable from the function
  return _links;
};
</script>

<template>
  <!-- Creates a new table of content navigation -->
  <nav class="toc">
    <!-- Creates the header "Table of contents" -->
    <header class="toc-header">
      <h3 class="text-xl font-bold">Table of contents</h3>
    </header>
    <!-- Creates a list of links below the header -->
    <ul class="toc-links">
      <!-- For each link, create a new link,
           with the id value as its key to make it unique;
           with the depth value as its class to ensure depth inside the table of contents;
           with the link to the corresponding headline;
           and with the text of the link. -->
      <li v-for="link of flattenLinks(links)" :key="link.id" :class="`toc-link _${link.depth}`">
        <a :href="`#${link.id}`">
          {{ link.text }}
        </a>
      </li>
    </ul>
  </nav>
</template>

<style scoped>
.toc {
  @apply p-4 bg-slate-50 border border-slate-200 rounded-lg;
  @apply max-h-[calc(100vh-6rem)] overflow-auto;
}

.toc-header {
  @apply pb-2 mb-2 border-b border-slate-200;
}

.toc-links {
  @apply flex flex-col gap-2 px-2;
}

.toc-link {
  @apply text-slate-500;
}

.toc-link._3 {
  @apply pl-3;
}

.toc-link._4 {
  @apply pl-6;
}

.toc-link._undefined {
  @apply pl-8;
}
</style>