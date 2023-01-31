<script setup lang="ts">
import { Camera, GltfModel, PointLight, Renderer, Scene } from 'troisjs';
import { onMounted, ref } from 'vue';

import SphereModel from '../assets/sphere.glb?url';

const renderer = ref();
const light = ref();

onMounted(() => {
  const pointerV3 = renderer.value.three.pointer.positionV3;
  renderer.value.onBeforeRender(() => {
    light.value.light.position.copy(pointerV3);
  });
});
</script>

<template>
  <Renderer
    ref="renderer"
    antialias
    :orbit-ctrl="{ enableDamping: true, autoRotate: true }"
    resize
    pointer
  >
    <Camera :position="{ x: 3, y: 0, z: 0 }" />
    <Scene>
      <PointLight
        ref="light"
        color="#646cff"
        :look-at="{ x: 0, y: 0, z: 0 }"
        :intensity="10"
        :position="{ x: 0, y: 0, z: 0 }"
      />
      <GltfModel :src="SphereModel" />
    </Scene>
  </Renderer>
</template>
