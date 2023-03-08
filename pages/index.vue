<template>
  <div class="flex justify-center gap-5">
    <Card
      v-for="(publication, i) in publications"
      :key="i"
      :publication="publication"
      :style="{
        opacity: target === null || target === i ? '1' : '0.25',
      }"
      @mouseover="target = i"
      @mouseout="target = null"
      class="transition-opacity duration-300"
    />

    <Button :action="test" />
    <Button link="/" />
    <Button />
  </div>
</template>

<script setup lang="ts">
import { Publication } from '~~/assets/types';

const publications = ref<Publication[]>(
  Array(5).fill({
    title: 'Test',
    description: 'test',
    image:
      'https://d28hgpri8am2if.cloudfront.net/book_images/onix/cvr9781421582696/my-hero-academia-vol-1-9781421582696_hr.jpg',
  })
);
const target = ref<number | null>(null);

const test = () => console.log('test');
</script>
