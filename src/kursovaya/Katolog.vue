<script setup>
import { ref, defineProps, defineEmits, computed } from 'vue';

const props = defineProps({
  products: Array,
});

const emit = defineEmits(['addToCart', 'openCart']);
const selectTovary = ref(null);
const cartCounts = ref({});
const searchQuery = ref(''); 

const tovary = [
  {
    id: 'perfume-1',
    name: 'L Air du Temps',
    price: 1000,
    description: 'Элегантный и изысканный. Верхние ноты: горечь гвоздики и палисандра. Ноты сердца: лепестки орхидеи. Базовые ноты: сочетание пикантных специй, кедра и ветивера.',
    image: '/duhi1.png', 
  },
  {
    id: 'perfume-2',
    name: 'Bleu de Chanel',
    price: 1100,
    description: 'Универсальный аромат. Верхние ноты: грейпфрут, лимон. Ноты сердца: iso e super, жасмин. Базовые ноты: белый мускус, ветивер.',
    image: '/duhi2.png',
  },
  {
    id: 'perfume-3',
    name: 'Black Opium',
    price: 1300,
    description: 'Многогранный и соблазнительный аромат. Верхние ноты: груша, розовый перец. Ноты сердца: кофе, жасмин. Базовые ноты: ваниль, пачули.',
    image: '/duhi3.png',
  },
  {
    id: 'perfume-4',
    name: 'Bal dafrique Byredo',
    price: 1200,
    description: 'Аромат сладкий и пьянящий. Верхние ноты: бергамот, чёрная смородина. Средние ноты: жасмин, цикламен. Базовые ноты: мускус, амбра.',
    image: '/duhi4.png',
  },
  {
    id: 'perfume-5',
    name: 'Tom Ford Lost Cherry',
    price: 1500,
    description: 'Смелый и сладкий аромат. Верхние ноты аромата: вишня, горький миндаль. Ноты сердца: вишня, слива. Базовые ноты: бензоин, бобы тонка, ваниль.',
    image: '/duhi5.png',
  },
  {
    id: 'perfume-6',
    name: 'Molecule 02',
    price: 1700,
    description: 'Дарит свежую прохладу и пронзительную нежность. Верхние ноты: амброксан, ветивер, ирис. Средние ноты: жасмин. Базовые ноты: амброксан, ветивер, ирис.',
    image: '/duhi6.png',
  },
];


const poisk = computed(() => {
  if (!searchQuery.value) {
    return tovary;
  }
  return tovary.filter((product) =>
    product.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
    product.description.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

function openProduct(product) {
  selectTovary.value = product;
}

function closeProduct() {
  selectTovary.value = null;
}

function goToCart() {
  emit('openCart');
}

function handleAddToCart(product) {
  if (!product.id) {
    product.id = `${product.name}-${product.price}`;
  }

  if (!cartCounts.value[product.id]) {
    cartCounts.value[product.id] = 1;
  } else {
    cartCounts.value[product.id]++;
  }

  emit('addToCart', { ...product });
}
</script>

<template>
  <div>
    
    <div v-if="selectTovary" class="product-detail-modal">
      <div class="overlay" @click="closeProduct"></div>
      <div class="product-detail">
        <button class="back-button" @click="closeProduct">Назад</button>
        <img :src="selectTovary.image" :alt="selectTovary.name" class="large-image" />
        <h2>{{ selectTovary.name }}</h2>
        <p><strong>Цена:</strong> {{ selectTovary.price }} ₽</p>
        <p>{{ selectTovary.description }}</p>
          <button @click="handleAddToCart(selectTovary)" class="add-to-cart-button">Добавить в корзину
            <span v-if="cartCounts[selectTovary.id]">({{ cartCounts[selectedProduct.id] }})</span>
          </button>
      </div>
    </div>

    
    <div v-else class="catalog">
      <h1 class="catalog-title">Каталог товаров</h1>

      <div class="search-container">
        <input 
          type="text" 
          v-model="searchQuery" 
          placeholder="Поиск по названию или описанию..." 
          class="search-input" 
        />
      </div>
      
      <div v-if="poisk.length === 0" class="no-products-message">
        Товары по вашему запросу не найдены.
      </div>

      <div class="product-list">
        <div
          v-for="product in poisk"
          :key="product.id"
          class="product-card">
          
          <img :src="product.image" :alt="product.name" class="product-image" />
          <h3 class="product-name">{{ product.name }}</h3>
          <p class="product-price">{{ product.price }} ₽</p>
          <button @click="openProduct(product)" class="view-button">Подробнее</button>
          <button @click="handleAddToCart(product)" class="add-to-cart-button">
            Добавить в корзину
            <span v-if="cartCounts[product.id]">({{ cartCounts[product.id] }})</span>
          </button>
        </div>
      </div>

      <button @click="goToCart" class="cart-button">Перейти в корзину</button>
    </div>
  </div>
</template>

<style scoped>
button {
  margin-top: 10px;
  padding: 10px 20px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  border-radius: 25px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #9a7a7a;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.catalog {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.catalog-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.search-container {
  position: sticky;
  top: 0;
  background-color: #fff;
  padding: 10px 0;
  width: 100%;
  z-index: 10;
}

.search-input {
  padding: 10px;
  width: 100%;
  max-width: 400px;
  margin-bottom: 20px;
  border-radius: 25px;
  border: 1px solid #ddd;
  font-size: 16px;
}

.no-products-message {
  color: #ff3333;
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 20px;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  padding-bottom: 20px;
}

.product-card {
  width: 30%;
  min-width: 250px;
  max-width: 300px;
  flex: 0 0 auto;
  margin: 10px;
  border-radius: 8px;
  overflow: hidden;
  background-color: rgba(255, 192, 203, 0.202);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
  height: 450px; 
}

.product-card:hover {
  transform: translateY(-5px);
}

.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-name {
  font-size: 18px;
  margin: 10px;
}

.product-price {
  color: #333;
  font-size: 16px;
  margin-bottom: 10px;
  text-align: center;
}

.view-button,
.add-to-cart-button {
  background-color: #f2d2d2;
  color: #333;
  width: 100%;
  padding: 12px;
  border-radius: 25px;
}

.add-to-cart-button {
  background-color: #d3a6a6;
}

.cart-button {
  background-color: #d3a6a6;
  color: rgb(0, 0, 0);
  width: 100%;
  padding: 12px;
  border-radius: 25px;
  margin-top: 20px;
}

.cart-button:hover {
  background-color: #9a7a7a;
}

.product-detail-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.product-detail {
  background-color: white;
  padding: 30px;
  max-width: 600px;
  width: 80%;
  border-radius: 8px;
  position: relative;
}

.large-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
  border-radius: 8px;
}

.back-button {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: transparent;
  color: #D3A6A6;
  border: none;
  font-size: 16px;
  cursor: pointer;
}

.back-button:hover {
  text-decoration: underline;
}
</style>
