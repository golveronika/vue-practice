<script setup lang="ts">
const props = withDefaults(
  defineProps<{
    items?: Array<{
      name: string
      value: string
    }>
    modelValue: string
  }>(),
  {},
)

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()
</script>

<template>
  <div class="select-container">
    <select
      @change="(event) => emit('update:modelValue', (event?.target as HTMLInputElement)?.value)"
    >
      <option v-for="item in props.items" :key="item.value" :value="item.value">
        {{ item.name }}
      </option>
    </select>
  </div>
</template>

<style scoped>
.select-container {
  border-radius: 5px;
  background-color: var(--color-purple);
  height: 38px;
  display: flex;
  align-items: center;
  background-image: url("data:image/svg+xml,%3Csvg width='9' height='5' viewBox='0 0 9 5' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M4.63077 4L1.26154 1' stroke='%23F7F7F7' stroke-linecap='round' stroke-linejoin='round'/%3E%3Cpath d='M8 1L4.63077 4' stroke='%23F7F7F7' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E%0A");
  background-repeat: no-repeat;
  background-position-x: calc(100% - 8px);
  background-position-y: center;
  background-size: 10px;
  cursor: pointer;
  select {
    cursor: pointer;
    padding: 8px;
    border: none;
    font-size: 18px;
    font-weight: medium;
    background-color: transparent;
    color: var(--color-white);
    appearance: none;
  }
}
</style>
