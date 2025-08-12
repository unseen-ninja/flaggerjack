<script setup>
import { ref, computed, watch, onMounted } from "vue"
import flagsData from "../data/flags.json"

const outlined = ref(true),
  flavour = ref("mocha"),
  downloadUrl = ref(""),
  selectedFlag = ref({ name: "Pride", file: "pride" }),
  countries = ref(flagsData)

const fileName = computed(() => {
  const f = (flavour.value || "default").toLowerCase().replace(/\s+/g, "-")
  const slug = selectedFlag.value?.file || "flag"
  const outline = outlined.value ? "-outlined" : ""
  return `flaggerjack-${f}-${slug}${outline}.png`
})

const flagPath = computed(() => {
  if (!selectedFlag.value) return ""
  return `/flags/${selectedFlag.value.file}.svg`
})

const loadImage = (src) =>
  new Promise((resolve, reject) => {
    const img = new Image()
    img.onload = () => resolve(img)
    img.onerror = () => reject(new Error(`Failed to load ${src}`))
    img.src = src
  })

async function draw() {
  const canvas = document.getElementById("flagppuccin")
  const ctx = canvas?.getContext("2d")
  if (!canvas || !ctx) return

  ctx.setTransform(1, 0, 0, 1, 0, 0)
  ctx.clearRect(0, 0, canvas.width, canvas.height)

  if (!flagPath.value) return

  try {
    const [pepperjackImg, flagImg, outlineImg, flagOverlay] = await Promise.all([
      flavour.value == "latte"
        ? loadImage("/flags/tpl-pepperjack-latte.png")
        : loadImage("/flags/tpl-pepperjack-mocha.png"),
      loadImage(flagPath.value),
      outlined.value
        ? flavour.value == "latte"
          ? loadImage("/flags/tpl-outline-latte.png")
          : loadImage("/flags/tpl-outline-mocha.png")
        : Promise.resolve(null),
      loadImage("/flags/tpl-flag-overlay.png"),
    ])

    if (outlineImg) {
      ctx.drawImage(outlineImg, 0, 0, canvas.width, canvas.height)
    }

    ctx.drawImage(pepperjackImg, 0, 0, canvas.width, canvas.height)

    ctx.save()
    const scaleFactor = 0.7
    const angle = (-15 * Math.PI) / 180
    ctx.rotate(angle)
    ctx.scale(scaleFactor, scaleFactor)
    ctx.drawImage(flagImg, -2, 15, canvas.width, canvas.height)
    ctx.drawImage(flagOverlay, -2, 15, canvas.width, canvas.height)
    ctx.restore()

    downloadUrl.value = canvas.toDataURL("image/png")
    fileName.value = createFileName()
  } catch (e) {
    console.error(e)
  } finally {
    // Ensure future draws start fresh
    ctx.setTransform(1, 0, 0, 1, 0, 0)
  }
}

watch([flagPath, outlined, flavour], draw)

onMounted(() => {
  draw()
})
</script>

<template>
  <div id="generator" class="md:self-center justify-self-center p-6">
    <div class="mb-12 p-8 bg-ctp-surface0 dark:bg-ctp-mantle rounded-xl">
      <canvas id="flagppuccin" width="256" height="256"></canvas>
    </div>

    <div class="flex gap-2 justify-content-center">
      <InputSwitch v-model="outlined" />
      <span class="text-ctp-text">Outline</span>
      <div class="ml-auto flex align-items-center">
        <RadioButton v-model="flavour" inputId="flavourLatte" name="flavour" value="latte" />
        <label for="flavourLatte" class="ml-2 text-ctp-text">Latte</label>
      </div>
      <div class="flex align-items-center">
        <RadioButton v-model="flavour" inputId="flavourMocha" name="flavour" value="mocha" />
        <label for="flavourMocha" class="ml-2 text-ctp-text">Mocha</label>
      </div>
    </div>
    <div class="mt-4 flex gap-12 items-center justify-content-center">
      <Dropdown
        v-model="selectedFlag"
        editable
        :options="countries"
        optionLabel="name"
        placeholder="Select a Flag"
        class="w-full md:w-14rem"
      />
    </div>

    <a
      v-if="downloadUrl"
      :href="downloadUrl"
      :download="fileName"
      class="block mt-4 w-full rounded-md bg-ctp-mantle hover:bg-ctp-mauve text-ctp-text hover:text-ctp-crust py-2 px-4 transition-all text-center"
    >
      Download
    </a>
  </div>
</template>
