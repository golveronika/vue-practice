<script setup lang="ts">
import TextInput from '@/components/TextInput.vue'
import SelectComponent from '@/components/SelectComponent.vue'
import DarkModeToggle from '@/components/DarkModeToggle.vue'
import ButtonComponent from '@/components/ButtonComponent.vue'
import ModalComponent from '@/components/ModalComponent.vue'
import AddTaskForm from '@/components/AddTaskForm.vue'
import TaskItem from '@/components/TaskItem.vue'

import ISearch from '@/assets/search.svg'
import IPlus from '@/assets/plus.svg'
import INotFound from '@/assets/not-found.svg'
import { computed, ref, watchEffect } from 'vue'
import type { ITask } from '@/types/index.ts'

const selectFilterValues = [
  { name: 'All', value: 'all' },
  { name: 'Completed', value: 'completed' },
  { name: 'Incompleted', value: 'incompleted' },
]

const searchFilter = ref<string>('')
const selectFilter = ref<string>('all')
const tasks = ref<Array<ITask>>(JSON.parse(window.localStorage.getItem('tasks') || '[]'))
const editedTask = ref<ITask | null>(null)
const showModal = ref<boolean>(false)

const filteredTasks = computed(() => {
  let result: Array<ITask> = tasks.value
  if (selectFilter.value === 'all') result = tasks.value
  else if (selectFilter.value === 'completed') result = tasks.value.filter((task) => task.completed)
  else if (selectFilter.value === 'incompleted')
    result = tasks.value.filter((task) => !task.completed)

  if (searchFilter.value) {
    result = result.filter((task) =>
      task.title.toLowerCase().includes(searchFilter.value.toLowerCase()),
    )
  }
  return result
})

const editTask = (task: ITask) => {
  const taskIndex = tasks.value.findIndex((t) => t.id === task.id)
  if (taskIndex !== -1) {
    tasks.value[taskIndex] = task // Обновите задачу в массиве
  }
  window.localStorage.setItem('tasks', JSON.stringify(tasks.value)) // Обновите localStorage
  if (showModal.value) showModal.value = false
  if (editedTask.value) editedTask.value = null
}

const addTask = (task: string) => {
  if (!task) return
  console.log('Task added:', task)
  const newTaskValue = {
    id: tasks.value.length + 1,
    title: task,
    completed: false,
  }
  tasks.value.push(newTaskValue)
  showModal.value = false
  window.localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

const removeTask = (task: ITask) => {
  const newTasksValue = tasks.value.filter((t) => t.id !== task.id)
  tasks.value = newTasksValue
  window.localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

const openTaskEditModal = (task: ITask) => {
  editedTask.value = task
  showModal.value = true
}
</script>

<template>
  <main class="container">
    <div class="container-inner">
      <h1 class="title">TODO LIST</h1>
      <div class="filters-wrapper">
        <TextInput placeholder="Search note..." :icon="ISearch" v-model="searchFilter" />
        <SelectComponent :items="selectFilterValues" v-model="selectFilter" />
        <DarkModeToggle />
      </div>
      <div class="tasks-wrapper">
        <template v-if="filteredTasks.length">
          <TaskItem
            v-for="task in filteredTasks"
            :key="task.id"
            :item="task"
            @edit-task="editTask"
            @open-task-edit-modal="openTaskEditModal"
            @remove-task="removeTask"
          />
        </template>
        <template v-else>
          <img :src="INotFound" alt="not-found" class="not-found" />
          <p>Empty...</p>
        </template>
      </div>

      <div class="add-container">
        <ButtonComponent variant="circle" @click="showModal = true">
          <img :src="IPlus" alt="plus-icon" />
        </ButtonComponent>
      </div>
    </div>
  </main>
  <ModalComponent v-if="showModal">
    <AddTaskForm
      @add-task="addTask"
      @edit-task="editTask"
      :task="editedTask"
      @cancel="showModal = false"
    />
  </ModalComponent>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  padding: 40px;

  .tasks-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    width: 100%;
    padding: 30px 0;
    width: 100%;
    max-width: 520px;
    margin: auto;
  }
  .container-inner {
    position: relative;
    max-width: 750px;
    width: 750px;
    min-height: calc(100vh - 80px);
  }
  .add-container {
    position: absolute;
    width: fit-content;
    bottom: 20px;
    right: 0;
  }
  .filters-wrapper {
    display: flex;
    width: 100%;
    gap: 16px;
  }
}
.title {
  text-align: center;
  font-size: 26px;
  font-weight: normal;
  margin-bottom: 18px;
}

@media screen and (max-width: 768px) {
  .filters-wrapper {
    flex-wrap: wrap;
  }
}
</style>
