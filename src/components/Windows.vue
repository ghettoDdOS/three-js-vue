<template>
  <Transition name="window">
    <div
      v-if="!isClosed"
      ref="window"
      class="window"
      style="position: fixed; cursor: move"
      :style="[style, windowSize]"
    >
      <div
        ref="titleBar"
        class="title-bar"
      >
        <div class="title-bar-text">My First VB4 Program</div>

        <div class="title-bar-controls">
          <button
            aria-label="Minimize"
            style="cursor: pointer"
            @click="isClosed = true"
          ></button>
          <button
            aria-label="Maximize"
            style="cursor: pointer"
            @click="maximizeWindow"
          ></button>
          <button
            aria-label="Close"
            style="cursor: pointer"
            @click="isClosed = true"
          ></button>
        </div>
      </div>
      <div class="window-body">
        <p>Hello, world!</p>
        <section
          class="field-row"
          style="justify-content: flex-end"
        >
          <button style="cursor: pointer">OK</button>
          <button style="cursor: pointer">Cancel</button>
        </section>
      </div>
    </div>
  </Transition>
  <button
    v-if="isClosed"
    @click="isClosed = false"
  >
    Reset
  </button>
</template>

<script setup lang="ts">
import { useDraggable } from '@vueuse/core';
import { reactive, ref } from 'vue';

const window = ref<HTMLElement | null>(null);
const titleBar = ref<HTMLElement | null>(null);

const isClosed = ref(false);
const isMaximized = ref(false);

const initialSize = {
  width: '250px',
  height: 'initial',
};
const windowSize = reactive(initialSize);

const { x, y, style } = useDraggable(window, {
  initialValue: { x: 40, y: 40 },
  handle: titleBar,
});

const maximizeWindow = () => {
  if (!window.value) return;
  if (isMaximized.value) {
    Object.assign(window.value.style, {
      top: x.value,
      left: y.value,
      ...initialSize,
    });
    isMaximized.value = false;
  } else {
    Object.assign(window.value.style, {
      top: 0,
      left: 0,
      width: '100%',
      height: '100%',
    });
    isMaximized.value = true;
  }
};
</script>

<style>
.window-enter-active,
.window-leave-active {
  transition: 0.5s ease;
}

.window-enter-from,
.window-leave-to {
  opacity: 0;
}
</style>
