<template>
  <div style="padding: 20px; font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; color: #333;">
    
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

    <section style="margin-bottom: 30px; padding: 15px; border: 1px solid #ccc; border-radius: 8px; background: #fff;">
      <h2>Задача 3 та 4: Список товарів з фільтрацією</h2>
      
      <div style="margin-bottom: 20px; padding: 10px; background: #f9f9f9; border-radius: 6px; display: flex; flex-direction: column; gap: 10px;">
        <div>
          <label style="display: block; margin-bottom: 4px; font-weight: bold;">Назва товару:</label>
          <input v-model="newTitle" type="text" placeholder="Введіть назву..." style="width: 100%; padding: 6px; box-sizing: border-box;" />
        </div>
        
        <div>
          <label style="display: block; margin-bottom: 4px; font-weight: bold;">Категорія (Тип):</label>
          <select v-model="newCategory" style="width: 100%; padding: 6px;">
            <option value="Tech">Електроніка</option>
            <option value="Clothes">Одяг</option>
          </select>
        </div>

        <button @click="addProduct" style="padding: 8px; background: #4caf50; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">
          Додати товар
        </button>
      </div>

      <div style="margin-bottom: 15px; display: flex; gap: 8px; align-items: center; flex-wrap: wrap; background: #eee; padding: 10px; border-radius: 6px;">
        <button @click="currentFilter = 'all'" :style="currentFilter === 'all' ? 'font-weight: bold;' : ''">Усі</button>
        <button @click="currentFilter = 'Tech'" :style="currentFilter === 'Tech' ? 'font-weight: bold;' : ''">Електроніка</button>
        <button @click="currentFilter = 'Clothes'" :style="currentFilter === 'Clothes' ? 'font-weight: bold;' : ''">Одяг</button>
        
        <span style="margin-left: auto; font-size: 14px;">
          Показано: <strong>{{ filteredProducts.length }}</strong> з <strong>{{ products.length }}</strong>
        </span>
      </div>

      <div style="display: grid; gap: 10px;">
        <p v-if="filteredProducts.length === 0" style="color: gray; font-style: italic; text-align: center; padding: 10px;">
          Нічого не знайдено за цим фільтром
        </p>
        
        <div 
          v-else
          v-for="p in filteredProducts" 
          :key="p.id" 
          :class="{ 'highlight-tech': p.category === 'Tech' }"
          style="padding: 12px; border: 1px solid #ddd; border-radius: 4px; display: flex; justify-content: space-between; align-items: center;"
        >
          <div><strong>{{ p.title }}</strong> — {{ p.category }}</div>
          <button @click="deleteProduct(p.id)" style="background: #e53935; color: white; border: none; padding: 4px 8px; border-radius: 4px; cursor: pointer;">Видалити</button>
        </div>
      </div>
    </section>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const isLoading = ref(false);
const hasError = ref(false);
const items = ref(['Елемент А', 'Елемент Б', 'Елемент В']);
const clearItems = () => { items.value = []; };
const addSampleItems = () => { items.value = ['Елемент А', 'Елемент Б', 'Елемент В']; };

const isPanelVisible = ref(true);

const products = ref([
  { id: 1, title: 'Монітор 24"', category: 'Tech' },
  { id: 2, title: 'Зимова куртка', category: 'Clothes' },
  { id: 3, title: 'Мишка бездротова', category: 'Tech' }
]);
const newTitle = ref('');
const newCategory = ref('Tech');
const currentFilter = ref('all');

const filteredProducts = computed(() => {
  if (currentFilter.value === 'all') return products.value;
  return products.value.filter(p => p.category === currentFilter.value);
});

const addProduct = () => {
  if (newTitle.value.trim() === '') return;
  products.value.push({
    id: Date.now(),
    title: newTitle.value,
    category: newCategory.value
  });
  newTitle.value = '';
};

const deleteProduct = (id) => {
  products.value = products.value.filter(p => p.id !== id);
};
</script>

<style scoped>
.highlight-tech {
  background-color: #e8f5e9 !important;
  border-color: #2e7d32 !important;
}
</style>