<script setup lang="ts">
import { defineProps, withDefaults } from 'vue'
const props = withDefaults(
  defineProps<{
    variant?: 'primary' | 'secondary' | 'circle' | 'unstyled'
  }>(),
  {
    variant: 'primary',
  },
)

const emit = defineEmits<{
  (e: 'click'): void
}>()
</script>

<template>
  <button class="button" :class="props.variant" @click="emit('click')">
    <slot></slot>
  </button>
</template>

<style scoped>
.button {
  background-color: var(--color-purple);
  border: 1px solid var(--color-purple);
  color: var(--color-white);
  border-radius: 5px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;

  &.unstyled {
    background-color: transparent;
    border: none;
    padding: 0;
  }
  &.secondary {
    background-color: var(--color-white);
    color: var(--color-purple);
    border: 1px solid var(--color-purple);
  }
  &.circle {
    border-radius: 50%;
    width: 50px;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
