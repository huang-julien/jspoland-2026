<template>
  <footer class="absolute bottom-0 left-0 right-0 px-14 pb-5 text-xs leading-snug opacity-60">
    <span v-if="label">{{ label }}&nbsp;</span><slot />
  </footer>
</template>

<script setup lang="ts">
withDefaults(
  defineProps<{
    /**
     * Prefix shown before the citation. Set to an empty string to omit it.
     */
    label?: string
  }>(),
  { label: 'Source:' },
)
</script>
