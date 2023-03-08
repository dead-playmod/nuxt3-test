<template>
  <div
    @mousemove="setRotation"
    @mouseover="isMouseOver = true"
    @mouseout="resetRotations"
    ref="card"
    class="group w-[12vw]"
  >
    <div
      :style="style"
      class="relative m-px flex aspect-[calc(2/3)] flex-col gap-3 rounded-md p-5 before:absolute before:inset-px before:-z-10 before:rounded-md before:bg-black"
      :class="{
        'transition-all': !isMouseOver,
        'duration-300': !isMouseOver,
      }"
    >
      <img
        :src="publication.image"
        :alt="`${publication.title} image`"
        class="rounded-md"
      />

      <span class="text-xl">{{ publication.title }}</span>

      <p>{{ publication.description }}</p>
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
  transform: `
    perspective(${perspective}rem)
    rotateX(${rotations.x}deg)
    rotateY(${rotations.y}deg)
    rotateZ(${rotations.z}deg)
    scale(${isMouseOver.value ? '1.03' : '1'})
  `,
  background: `white radial-gradient(circle, rgba(0,0,0,1) 1%, rgba(255,255,255,1) 40%)`,
  backgroundSize: isMouseOver.value ? '1500px 1500px' : '4000px 4000px',
  backgroundPosition: `calc(${rotations.y}px * 18 + 50%) calc(${rotations.x}px * -20 + 50%)`,
  backgroundRepeat: 'no-repeat',
}));

const setRotation = (event: MouseEvent) => {
  if (!card.value) return;

  const { width, height, x, y } = card.value.getBoundingClientRect();
  const mouseX = event.clientX - x;
  const mouseY = event.clientY - y;

  rotations.x = (mouseY - height / 2) * strength.x;
  rotations.y = -(mouseX - width / 2) * strength.y;

  console.log(mouseX);
};

const resetRotations = () => {
  isMouseOver.value = false;

  rotations.x = 0;
  rotations.y = 0;
  rotations.z = 0;
};

onMounted(() => {
  const rec = card.value?.getBoundingClientRect();
  console.log('rec', rec);
});
</script>
