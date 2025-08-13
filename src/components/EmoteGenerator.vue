<script setup>
import { ref, computed } from "vue"
import flagsData from "../data/flags.json"
import Controls from "./GeneratorControls.vue"
import PreviewCanvas from "./GeneratorPreview.vue"

const outlined = ref(true)
const flavour = ref("mocha") // "latte" | "mocha"
const selectedFlag = ref({ name: "Pride", file: "pride" })
const countries = ref(flagsData)
const downloadUrl = ref("")

const fileName = computed(() => {
  const f = (flavour.value || "default").toLowerCase().replace(/\s+/g, "-")
  const slug = selectedFlag.value?.file || "flag"
  const outline = outlined.value ? "-outlined" : ""
  return `flaggerjack-${f}-${slug}${outline}.png`
})

function handleRendered(url) {
  downloadUrl.value = url
}
</script>

<template>
  <div id="preview" class="p-8 grid place-items-center">
    <PreviewCanvas :outlined="outlined" :flavour="flavour" :selected-flag="selectedFlag" @rendered="handleRendered" />
  </div>

  <div id="controls" class="flex flex-col lg:bg-ctp-mantle lg:p-8">
    <Controls
      v-model:outlined="outlined"
      v-model:flavour="flavour"
      v-model:selected-flag="selectedFlag"
      :countries="countries"
    />

    <a
      v-if="downloadUrl"
      :href="downloadUrl"
      :download="fileName"
      class="block mt-6 lg:mt-auto w-full rounded-md bg-ctp-surface0 hover:bg-ctp-mauve text-ctp-text hover:text-ctp-crust py-2 px-4 transition-all text-center"
    >
      Download Image
    </a>
  </div>
</template>
