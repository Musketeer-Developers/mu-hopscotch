<template>
  <div class="flex flex-col items-center justify-center p-4">
    <img
      v-if="src"
      :src="src"
      loading="lazy"
      class="inline-flex flex-col object-contain object-center mb-4"
      :class="large ? 'w-32 h-32' : 'w-16 h-16'"
      :alt="alt"
    />
    <slot name="icon"></slot>
    <span v-if="heading" class="mb-2 font-semibold text-center">
      {{ heading }}
    </span>
    <span
      class="max-w-sm mb-4 text-center whitespace-normal text-secondaryLight"
    >
      {{ text }}
    </span>
    <slot></slot>
  </div>
</template>

<script setup lang="ts">
withDefaults(
  defineProps<{
    src?: string
    alt?: string
    heading?: string
    text?: string
    large?: boolean
  }>(),
  {
    alt: "",
    text: "",
  }
)
</script>
