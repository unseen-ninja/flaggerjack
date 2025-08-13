<script setup>
import { computed, watch, onMounted } from "vue"

const props = defineProps({
  outlined: { type: Boolean, required: true },
  flavour: { type: String, required: true }, // "latte" | "mocha"
  selectedFlag: { type: Object, required: true }, // { name, file }
})
const emit = defineEmits(["rendered"])

const flagPath = computed(() => (props.selectedFlag ? `/flags/${props.selectedFlag.file}.svg` : ""))

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
      props.flavour === "latte"
        ? loadImage("/flags/tpl-pepperjack-latte.png")
        : loadImage("/flags/tpl-pepperjack-mocha.png"),
      loadImage(flagPath.value),
      props.outlined
        ? props.flavour === "latte"
          ? loadImage("/flags/tpl-outline-latte.png")
          : loadImage("/flags/tpl-outline-mocha.png")
        : Promise.resolve(null),
      loadImage("/flags/tpl-flag-overlay.png"),
    ])

    if (outlineImg) ctx.drawImage(outlineImg, 0, 0, canvas.width, canvas.height)
    ctx.drawImage(pepperjackImg, 0, 0, canvas.width, canvas.height)

    ctx.save()
    const scaleFactor = 0.7
    const angle = (-15 * Math.PI) / 180
    ctx.rotate(angle)
    ctx.scale(scaleFactor, scaleFactor)
    ctx.drawImage(flagImg, -2, 25, canvas.width, canvas.height)
    ctx.drawImage(flagOverlay, -2, 25, canvas.width, canvas.height)
    ctx.restore()

    emit("rendered", canvas.toDataURL("image/png"))
  } catch (e) {
    console.error(e)
  } finally {
    ctx.setTransform(1, 0, 0, 1, 0, 0)
  }
}

watch(() => [flagPath.value, props.outlined, props.flavour, props.selectedFlag?.file], draw)
onMounted(draw)
</script>

<template>
  <canvas id="flagppuccin" class="w-[256px] aspect-square lg:w-[512px]" width="512" height="512"></canvas>
</template>
