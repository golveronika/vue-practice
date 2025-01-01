<script setup lang="ts">
import { defineProps, onMounted, ref, withDefaults } from 'vue'
const inputRef = ref<HTMLInputElement | null>(null)

const props = withDefaults(
  defineProps<{
    placeholder?: string
    icon?: string
    modelValue: string
  }>(),
  {
    placeholder: '',
  },
)
onMounted(() => {
  inputRef.value?.focus()
})
</script>

<template>
  <div class="input-container">
    <input
      ref="inputRef"
      type="text"
      class="input"
      :class="{ 'with-icon': props.icon }"
      :placeholder="props.placeholder"
      :value="modelValue"
      @input="$emit('update:modelValue', ($event?.target as HTMLInputElement)?.value)"
    />
    <img v-if="props.icon" class="icon" :src="icon" alt="input-icon" />
  </div>
</template>

<style scoped>
.input-container {
  flex: 1;
  position: relative;
  height: 38px;
  .icon {
    position: absolute;
    right: 16px;
    top: 50%;
    transform: translateY(-50%);
  }
  .input {
    width: 100%;
    font-size: 16px;
    padding: 11px 16px;
    height: 38px;
    border: 1px solid var(--color-purple);
    border-radius: 5px;
    min-width: 200px;
    &.with-icon {
      padding-right: 40px;
    }
    &::placeholder {
      color: var(--color-purple);
      opacity: 0.5;
    }
  }
}

html.dark {
  .input-container {
    .input {
      border: 1px solid var(--color-white);
      &::placeholder {
        color: var(--color-white);
      }
    }
    .icon {
      filter: brightness(0) saturate(100%) invert(98%) sepia(98%);
    }
  }
}
</style>
