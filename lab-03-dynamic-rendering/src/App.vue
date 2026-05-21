<template>
  <div style="padding: 20px; font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
    <h2>Задача 1: Стани сторінки</h2>
    
    <div style="margin-bottom: 15px; display: flex; gap: 8px; flex-wrap: wrap;">
      <button @click="isLoading = !isLoading">Toggle Loading</button>
      <button @click="hasError = !hasError">Toggle Error</button>
      <button @click="clearItems">Clear Items</button>
      <button @click="addSampleItems">Add Sample Items</button>
    </div>

    <div style="padding: 15px; background: #f5f5f5; border-radius: 4px; min-height: 50px;">
      <div v-if="isLoading" style="font-weight: bold; color: #0288d1;">Завантаження…</div>
      <div v-else-if="hasError" style="color: #d32f2f; font-weight: bold;">Помилка завантаження</div>
      <div v-else-if="items.length === 0" style="color: #757575; font-style: italic;">Немає даних</div>
      <ul v-else style="margin: 0; padding-left: 20px;">
        <li v-for="(item, index) in items" :key="index">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const isLoading = ref(false);
const hasError = ref(false);
const items = ref(['Елемент А', 'Елемент Б', 'Елемент В']);

const clearItems = () => { items.value = []; };
const addSampleItems = () => { items.value = ['Елемент А', 'Елемент Б', 'Елемент В']; };
</script>