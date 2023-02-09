<script setup lang="ts">
import { useScroll } from '@vueuse/core';
import { computed, ref, watch, watchEffect } from 'vue';

const loadImage = (src: string): Promise<HTMLImageElement> =>
  new Promise((resolve, reject) => {
    const image = new Image();
    image.src = src;

    image.onload = () => resolve(image);
    image.onerror = (e) => reject(e);
  });

const images = await Promise.all(
  [...Array(90).keys()].map((i) =>
    loadImage(
      `https://leaders2022.innoagency.ru/src/media/frameswebp/flown_${String(
        i
      ).padStart(2, '0')}.webp`
    )
  )
);

const { y: scrollPosition, directions: scrollDirections } = useScroll(window);

const canvas = ref<HTMLCanvasElement>();
const container = ref<HTMLDivElement>();
const currentImageIndex = ref(0);

const ctx = computed(() => canvas.value?.getContext('2d'));

const init = () => window.scroll({ top: 0 });

watch(scrollPosition, (current, old) => {
  if (
    scrollDirections.bottom &&
    currentImageIndex.value !== images.length - 1
  ) {
    currentImageIndex.value++;
  } else if (scrollDirections.top && currentImageIndex.value !== 0) {
    currentImageIndex.value--;
  }
});

watchEffect(
  () =>
    !!ctx.value &&
    ctx.value.drawImage(images[currentImageIndex.value], 0, 0, 2000, 1280)
);

// const animation = () => {
//   const canv = document.getElementById('background');
//   const context = canv.getContext('2d');
//   const setImage = function (newLocation) {
//     context.drawImage(images[newLocation], 0, 0, 2000, 1280);
//   };
//   const scrollHandler = function () {
//     let currentOffset = window.pageYOffset;
//     let winH = window.innerHeight;
//     let winW = window.innerWidth;
//     currentLocation = Math.round(
//       (currentOffset / (container.clientHeight - winH)) * totalImages
//     );
//     if (currentLocation < 0) currentLocation = 0;
//     if (currentLocation >= images.length) currentLocation = images.length - 1;
//     if (currentOffset > container.scrollHeight - canv.scrollHeight) {
//       canv.style.top = container.scrollHeight - canv.scrollHeight + 'px';
//       canv.style.position = 'absolute';
//     } else {
//       canv.style.top = '0px';
//       canv.style.position = 'fixed';
//     }
//     if (currentOffset <= container.scrollHeight) setImage(currentLocation);
//     let scheduleParams = schedule.getBoundingClientRect();

//     if (
//       scheduleParams.top - winH + scheduleParams.height > 0 &&
//       scheduleParams.top - winH < 0
//     ) {
//       let k =
//         0.8 + 0.2 * ((scheduleParams.top - winH) / -scheduleParams.height);
//       schedule.style.backgroundSize = `auto ${
//         scheduleParams.height * (winH > winW ? k / 4 : k)
//       }px`;
//     }
//     if (currentOffset > container.scrollHeight) {
//       if (currentOffset < container.scrollHeight + 700) {
//         let k = 0.45 + (0.6 * (currentOffset - container.scrollHeight)) / 700;
//         header.style.background = `rgba(23, 21, 54, ${k})`;
//       }
//     } else {
//       header.style.background = `rgba(23, 21, 54, ${0.45})`;
//     }
//   };
//   const canvasFillWin = function (e) {
//     var h = 1280;
//     var w = 2000;
//     var ratio = h / w;
//     var winW = window.innerWidth;
//     var winH = window.innerHeight;
//     var winRatio = winH / winW;

//     if (winRatio > ratio) {
//       canv.style.width = '156.25vh';
//       canv.style.height = '100vh';
//       canv.style.left = 'calc(-100vh * 1.5625 / 2 + 50vw)';
//     } else {
//       canv.style.width = '100vw';
//       canv.style.height = '64vw';
//       canv.style.left = '0px';
//     }
//   };
//   window.addEventListener('scroll', scrollHandler);
//   window.addEventListener('resize', canvasFillWin, false);
//   canvasFillWin();
// };
</script>

<template>
  <div
    class="container"
    :style="{ height: 'calc(100vh + 2000px)' }"
    @load="init"
  >
    <canvas
      ref="canvas"
      :width="2000"
      height="1280"
      :style="{
        width: '156.25vh',
        height: '100vh',
        position: 'fixed',
        top: 0,
        left: 'calc(-78.125vh + 50vw)',
      }"
    ></canvas>
  </div>
</template>
