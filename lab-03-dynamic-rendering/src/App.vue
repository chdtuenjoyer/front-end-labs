<template>
  <div style="padding: 20px; font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
    
    <section style="margin-bottom: 30px; padding: 15px; border: 1px solid #ccc; border-radius: 8px;">
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
    </section>

    <section style="margin-bottom: 30px; padding: 15px; border: 1px solid #ccc; border-radius: 8px;">
      <h2>Задача 2: v-if vs v-show</h2>
      <button @click="isPanelVisible = !isPanelVisible" style="margin-bottom: 15px; padding: 6px 12px; cursor: pointer;">
        Показати/Сховати панель
      </button>

      <div style="display: flex; gap: 15px; flex-wrap: wrap;">
        <div style="flex: 1; min-width: 200px;">
          <h3>Панель через v-if</h3>
          <div v-if="isPanelVisible" style="padding: 15px; background: #e0f7fa; border: 1px solid #00acc1; border-radius: 4px;">
            Цей блок повністю зникає і з'являється у дереві DOM сторінки.
          </div>
        </div>

        <div style="flex: 1; min-width: 200px;">
          <h3>Панель через v-show</h3>
          <div v-show="isPanelVisible" style="padding: 15px; background: #fff3e0; border: 1px solid #fb8c00; border-radius: 4px;">
            Цей блок завжди присутній в DOM, але приховується через CSS стиль "display: none".
          </div>
        </div>
      </div>
    </section>

  </div>
</template>

<script setup>
import { ref } from 'vue';

const isLoading = ref(false);
const hasError = ref(false);
const items = ref(['Елемент А', 'Елемент Б', 'Елемент В']);

const clearItems = () => { items.value = []; };
const addSampleItems = () => { items.value = ['Елемент А', 'Елемент Б', 'Елемент В']; };

const isPanelVisible = ref(true);
</script>