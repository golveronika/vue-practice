<script setup lang="ts">
import { ref, watchEffect } from 'vue'
import IDark from '@/assets/dark.svg'
import ILight from '@/assets/light.svg'

const isDark = ref(false)

function updateTheme() {
  if (isDark.value) {
    document.documentElement.classList.add('dark')
    document.documentElement.style.colorScheme = 'dark'
  } else {
    document.documentElement.classList.remove('dark')
    document.documentElement.style.colorScheme = 'light'
  }
}
watchEffect(updateTheme)
</script>

<template>
  <label class="dark-mode-container" for="dark-mode-toggle">
    <img class="icon" :src="isDark ? ILight : IDark" alt="sun-icon" />
    <input
      type="checkbox"
      id="dark-mode-toggle"
      :checked="isDark"
      v-model="isDark"
      class="dark-mode-input"
    />
  </label>
</template>

<style scoped>
.dark-mode-container {
  position: relative;
  cursor: pointer;
  height: 38px;
  padding: 11px 16px;
  height: 38px;
  border: 1px solid var(--color-purple);
  border-radius: 5px;
  background-color: var(--color-purple);
  width: 38px;
  height: 38px;
  .dark-mode-input {
    visibility: hidden;
  }

  .icon {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>
