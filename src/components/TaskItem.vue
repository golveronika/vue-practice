<script setup lang="ts">
import { defineProps, ref, withDefaults } from 'vue'
import type { ITask } from '@/types/index.ts'
import CheckboxInput from '@/components/CheckboxInput.vue'
import ButtonComponent from '@/components/ButtonComponent.vue'

import IEdit from '@/assets/edit.svg'
import ITrash from '@/assets/trash.svg'

const { item } = defineProps<{
  item: ITask
}>()

const emit = defineEmits<{
  (event: 'edit-task', task: ITask): void
  (event: 'open-task-edit-modal', task: ITask): void
  (event: 'remove-task', task: ITask): void
}>()

const handleComplete = () => {
  isChecked.value = !isChecked.value
  emit('edit-task', {
    ...item,
    completed: isChecked.value,
  })
}
const isChecked = ref(item.completed)
</script>

<template>
  <div class="task-item-wrapper">
    <div class="task-item" @click="handleComplete">
      <CheckboxInput v-model:checked="isChecked" @change="handleComplete" />
      <p class="task-title" :class="{ completed: isChecked }">{{ item.title }}</p>
    </div>
    <div class="task-actions">
      <ButtonComponent variant="unstyled" @click="emit('open-task-edit-modal', item)">
        <img class="action-icon" :src="IEdit" alt="edit-icon" />
      </ButtonComponent>
      <ButtonComponent variant="unstyled" @click="emit('remove-task', item)">
        <img class="action-icon" :src="ITrash" alt="edit-icon" />
      </ButtonComponent>
    </div>
  </div>
</template>

<style scoped>
.task-item {
  display: flex;
  padding: 17px 0;
  cursor: pointer;
  .task-title {
    margin-left: 17px;
    width: 100%;
    &.completed {
      text-decoration: line-through;
      opacity: 0.5;
    }
  }
}
.task-item-wrapper {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  &:not(:last-child) {
    border-bottom: 1px solid var(--color-purple);
  }
  &:hover {
    .task-actions {
      opacity: 1;
    }
  }
  .task-actions {
    display: flex;
    gap: 10px;
    opacity: 0;
    .action-icon {
      filter: grayscale(1);
      opacity: 0.6;
      &:hover {
        filter: none;
        opacity: 1;
      }
    }
  }
}
</style>
