<template>
  <button v-if="action" @click="action">button</button>
  <NuxtLink v-else :to="link">link</NuxtLink>
</template>

<script setup lang="ts">
type action = {
  action?: (e: MouseEvent) => void;
  link?: string;
};

const { action, link } = withDefaults(defineProps<action>(), {
  link: '/',
});
</script>
