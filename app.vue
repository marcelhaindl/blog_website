<template>
  <nav role="main">
    <h2 class="font-pacifico flex-1 text-indigo-200 font-normal">Framework Coding</h2>
    <div>
      <NuxtLink to="/">Home</NuxtLink>
      <NuxtLink to="/soundboard">Soundboard</NuxtLink>
      <NuxtLink to="/game">Game</NuxtLink>
      <NuxtLink to="/blog">Blog</NuxtLink>
    </div>
    <div class="profile">
      <img src="/images/profile.png" alt="Profile" height="44" width="44">
    </div>
  </nav>
  <NuxtPage />
</template>

<style>
nav[role='main'] {
  @apply 2xl:px-64 xl:px-48 lg:px-36 md:px-24 sm:px-16 px-12 h-20 w-full flex flex-row justify-between items-center bg-neutral-800 text-neutral-400 fixed;
}

nav[role='main'] > div {
  @apply flex flex-row justify-center gap-x-6
}

nav[role='main'] > div > a {
  @apply px-4 py-1 text-neutral-400 hover:text-neutral-300 rounded;
}

nav[role='main'] > div > a.router-link-active {
  @apply text-neutral-50 underline underline-offset-4;
}

.profile {
  @apply flex flex-1 flex-row items-center;
  justify-content: flex-end !important;
}
</style>