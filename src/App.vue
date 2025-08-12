<script setup lang="ts">
import EmoteCreator from "./components/EmoteCreator.vue"
import FooterNote from "./components/FooterNote.vue"
</script>

<template>
  <main id="main-content" class="grid gap-6 min-h-screen">
    <FooterNote />
    <EmoteCreator />
  </main>
</template>

<style>
#main-content {
  grid-template-rows: min-content 1fr;
}

@media (width > 500px) {
  #main-content {
    grid-template-rows: 1fr min-content;
  }

  #main-content :first-child {
    grid-row: 2 / -1;
  }
}
</style>
