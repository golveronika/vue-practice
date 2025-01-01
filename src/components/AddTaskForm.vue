<script setup lang="ts">
import TextInput from '@/components/TextInput.vue'
import ButtonComponent from '@/components/ButtonComponent.vue'

import { ref } from 'vue'
import type { ITask } from '@/types'

const { task } = defineProps<{
  task: ITask | null
}>()

const newTask = ref<string>(task?.title || '')

const emit = defineEmits<{
  (event: 'add-task', task: string): void
  (event: 'edit-task', task: ITask): void
  (event: 'cancel'): void
}>()

const handleApply = () => {
  if (task) {
    const updatedTask = { ...task, title: newTask.value }
    emit('edit-task', updatedTask)
  } else {
    emit('add-task', newTask.value)
  }
}
</script>
<template>
  <div class="modal-content">
    <h1 v-if="!task" class="modal-title">New Note</h1>
    <h1 v-else class="modal-title">Edit Note</h1>
    <TextInput placeholder="Input your note..." v-model="newTask" />
    <div class="buttons">
      <ButtonComponent variant="secondary" @click="emit('cancel')">CANCEL</ButtonComponent>
      <ButtonComponent variant="primary" @click="handleApply">APPLY</ButtonComponent>
    </div>
  </div>
</template>

<style scoped>
.modal-content {
  width: 100%;
  display: flex;
  flex-direction: column;
  .buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 128px;
  }
  .modal-title {
    text-align: center;
    font-size: 24px;
    font-weight: medium;
    text-transform: uppercase;
    width: 100%;
    margin-bottom: 25px;
  }
}
</style>
