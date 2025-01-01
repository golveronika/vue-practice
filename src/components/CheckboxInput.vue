<script setup lang="ts">
import { defineProps, withDefaults } from 'vue'
import ICheck from '@/assets/check.svg'

const props = withDefaults(
  defineProps<{
    checked?: boolean
  }>(),
  {},
)

const emit = defineEmits<{
  (event: 'input:checked', value: boolean): void
}>()

const handleChange = () => {
  emit('input:checked', !props.checked) // Передаём новое значение родителю
}
</script>

<template>
  <label class="checkbox" :class="{ checked: props.checked }">
    <img v-if="checked" :src="ICheck" alt="check-icon" />
    <input class="checkbox-input" type="checkbox" :checked="props.checked" @input="handleChange" />
  </label>
</template>

<style scoped>
.checkbox {
  width: 26px;
  min-width: 26px;
  height: 26px;
  background-color: var(--color-white);
  border: 1px solid var(--color-purple);
  border-radius: 2px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  &.checked {
    background-color: var(--color-purple);
  }
}

.checkbox-input {
  visibility: hidden;
  width: 1px;
  height: 1px;
}
</style>
