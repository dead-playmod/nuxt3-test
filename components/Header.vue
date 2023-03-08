<template>
  <header>
    <span>{{ title }}</span>
  </header>
</template>

<script setup lang="ts">
const props = withDefaults(defineProps<{ title?: string }>(), {
  title: 'Header title',
});
</script>
