<script setup>
const outlined = defineModel("outlined", { type: Boolean, default: true })
const flavour = defineModel("flavour", { type: String, default: "mocha" })
const selectedFlag = defineModel("selected-flag", { type: Object })

const props = defineProps({
  countries: { type: Array, required: true },
})
</script>

<template>
  <div class="flex gap-6 flex-col lg:flex-col-reverse">
    <h2 class="hidden lg:block text-ctp-mauve text-lg font-bold lg:order-last">Settings</h2>

    <div class="flex gap-4">
      <span class="text-ctp-text">Outline</span>
      <InputSwitch v-model="outlined" />
    </div>

    <div class="flex gap-4">
      <div class="flex gap-2 items-center">
        <label for="flavourLatte" class="text-ctp-text">Latte</label>
        <RadioButton v-model="flavour" inputId="flavourLatte" name="flavour" value="latte" />
      </div>
      <div class="flex gap-2 items-center">
        <label for="flavourMocha" class="text-ctp-text">Mocha</label>
        <RadioButton v-model="flavour" inputId="flavourMocha" name="flavour" value="mocha" />
      </div>
    </div>

    <Dropdown
      v-model="selectedFlag"
      editable
      :options="props.countries"
      optionLabel="name"
      placeholder="Select a Flag"
      class="w-full md:w-14rem"
    />
  </div>
</template>
