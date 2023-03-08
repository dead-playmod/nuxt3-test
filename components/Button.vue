<template>
  <button v-if="action" @click="action">button</button>
  <NuxtLink v-else :to="link">link</NuxtLink>
</template>

<script setup lang="ts">
const { action, link } = withDefaults(
  defineProps<{
    action?: (e: MouseEvent) => void;
    link?: string;
  }>(),
  { link: '/' }
);
</script>
