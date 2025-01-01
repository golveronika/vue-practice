<script setup lang="ts"></script>

<template>
  <div class="modal-overlay">
    <div class="modal-wrapper">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 8px;
  .modal-wrapper {
    background-color: var(--color-white);
    width: 500px;
    padding: 18px 30px;
    border-radius: 16px;
  }
}
</style>
