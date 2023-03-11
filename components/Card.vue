<template>
  <div
    @mousemove="setRotation"
    @mouseover="isMouseOver = true"
    @mouseleave="isMouseOver = false"
    ref="card"
    class="group w-[12vw]"
  >
    <div
      :style="style"
      class="relative m-px flex w-full flex-col gap-3 overflow-hidden rounded-md p-5 before:absolute before:inset-px before:-z-10 before:rounded-md before:bg-black"
      :class="{
        'transition-all': !isMouseOver,
        'duration-300': !isMouseOver,
      }"
    >
      <img
        :src="publication.image"
        :alt="`${publication.title} image`"
        class="z-10 aspect-[calc(2/3)] rounded-md"
      />

      <span class="text-xl">{{ publication.title }}</span>

      <p class="flex">{{ publication.description }}</p>

      <div
        :style="glintStyle"
        class="absolute inset-px rounded-md border border-black transition-opacity duration-500"
      ></div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Publication } from '~~/assets/types';

const { publication } = defineProps<{ publication: Publication }>();

const card = ref<HTMLElement>();

const perspective = 90;
const strength = { x: 0.05, y: 0.06 };

const isMouseOver = ref(false);
const rotations = reactive({
  x: 0,
  y: 0,
  z: 0,
});

const style = computed(() => ({
  transform: isMouseOver.value
    ? `
      perspective(${perspective}rem)
      rotateX(${rotations.x}deg)
      rotateY(${rotations.y}deg)
      rotateZ(${rotations.z}deg)
      scale(1.03)
      translateY(-5px)
    `
    : `
      rotateX(0)
      rotateY(0)
      rotateZ(0)
      scale(1)
      translateY(0)
    `,
  background: `white radial-gradient(circle, rgba(0,0,0,1) 1%, rgba(255,255,255,1) 35%)`,
  backgroundSize: isMouseOver.value ? '3000px 3000px' : '5000px 5000px',
  backgroundPosition: isMouseOver.value
    ? `calc(${rotations.y}px * 18 + 50%) calc(${rotations.x}px * -20 + 50%)`
    : 'center',
  backgroundRepeat: 'no-repeat',
}));
const glintStyle = computed(() => ({
  background: `linear-gradient(
      ${20 + rotations.y * rotations.x * 0.1}deg,
      rgba(255,255,255,0.05) ${25 - rotations.x * 0.8}%,
      rgba(255,255,255,0.2) ${35 - rotations.x * 0.6}%,
      rgba(255,255,255,0.6) ${47 - rotations.x * 0.4}%,
      rgba(255,255,255,0.6) ${53 - rotations.x * 0.4}%,
      rgba(255,255,255,0.2) ${65 - rotations.x * 0.6}%,
      rgba(255,255,255,0.05) ${75 - rotations.x * 0.8}%
    )`,
  backgroundOrigin: 'content-box',
  opacity: isMouseOver.value ? 0.8 : 0,
}));

const setRotation = (event: MouseEvent) => {
  if (!card.value) return;

  const { width, height, x, y } = card.value.getBoundingClientRect();
  const mouseX = event.clientX - x;
  const mouseY = event.clientY - y;

  rotations.x = (mouseY - height / 2) * strength.x;
  rotations.y = -(mouseX - width / 2) * strength.y;
};

const resetRotations = () => {
  rotations.x = 0;
  rotations.y = 0;
  rotations.z = 0;
};

watch(
  () => isMouseOver.value,
  () => {
    if (isMouseOver.value) resetRotations();
  }
);
</script>
