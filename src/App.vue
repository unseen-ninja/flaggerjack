<script setup lang="ts">
import EmoteGenerator from "./components/EmoteGenerator.vue"
import FooterNote from "./components/FooterNote.vue"
</script>

<template>
  <main id="generator" class="grid max-lg:gap-8 lg:grid-cols-3 p-8 lg:p-0 max-lg:max-w-xl min-h-screen max-lg:mx-auto">
    <EmoteGenerator />
    <FooterNote />
  </main>
</template>

<style>
#generator {
  grid-template-rows: repeat(3, min-content);
  grid-auto-rows: none;
  grid-template-areas:
    "footer"
    "preview"
    "controls";
}

#preview {
  grid-area: preview;
}
#controls {
  grid-area: controls;
}

#disclaimer {
  grid-area: footer;
}

@media (width >= 64rem) {
  #generator {
    grid-template-rows: 1fr min-content;
    grid-template-columns: max(360px) 1fr;
    grid-template-areas:
      "controls preview"
      "footer preview";
  }
}
</style>
