<script setup>
import { ref, computed, defineEmits, defineProps } from "vue";

const props = defineProps({
  products: Array,
  cart: {
    type: Array,
    required: true,
  },
  user: {
    type: Object,
    default: null,
  },
});

const emit = defineEmits(["updateCart", "openCatalog", "addToCart"]);

const totalPrice = computed(() =>
  props.cart.reduce((sum, item) => sum + item.price * item.quantity, 0)
);

function updateQuantity(product, change) {
  const updatedCart = props.cart.map(item => ({ ...item })); 
  const index = updatedCart.findIndex(item => item.id === product.id);
  if (index !== -1) {
    updatedCart[index].quantity += change;

    if (updatedCart[index].quantity <= 0) {
      updatedCart.splice(index, 1);
    }
  }

  emit("updateCart", updatedCart);
}

function clearCart() {
  emit("updateCart", []); 
}

const showSuccessModal = ref(false);
const showAuthModal = ref(false);

function placeOrder() {
  if (props.user) {
    showSuccessModal.value = true;
    emit("updateCart", []); 
  } else {
    showAuthModal.value = true;
  }
}

function closeModals() {
  showSuccessModal.value = false;
  showAuthModal.value = false;
}

const cartCounts = ref({});

function handleAddToCart(product) {
  if (!cartCounts.value[product.id]) {
    cartCounts.value[product.id] = 1;
  } else {
    cartCounts.value[product.id]++;
  }
  emit("addToCart", product);
}

const selectedProduct = ref(null);
const selectedIndex = ref(null); 
function openProduct(product, index) {
  selectedProduct.value = product;
  selectedIndex.value = index;
}

function closeProduct() {
  selectedProduct.value = null;
  selectedIndex.value = null;
}

function goToCart() {
  emit("openCatalog");
}
</script>

<template>
  <div>
    
    <div v-if="selectedProduct" class="product-detail-modal">
      <div class="overlay" @click="closeProduct"></div>
      <div class="product-detail">
        <button class="back-button" @click="closeProduct">Назад</button>
        <img :src="`duhi${selectedProduct.id.replace('perfume-', '')}.png`" :alt="selectedProduct.name" class="large-image" />
        <h2>{{ selectedProduct.name }}</h2>
        <p><strong>Цена:</strong> {{ selectedProduct.price }} ₽</p>
        <p>{{ selectedProduct.description }}</p>
        <button @click="handleAddToCart(selectedProduct)" class="add-to-cart-button">Добавить в корзинуs
        <span v-if="cartCounts[selectedProduct.id]">({{ cartCounts[selectedProduct.id] }})</span>
        </button>
      </div>
    </div>

    <div v-else class="catalog">
      <div class="product-list">
        <div v-for="product in products" :key="product.id" class="product-card">
          <img :src="`duhi${product.id.replace('perfume-', '')}.png`" :alt="product.name" class="product-image"/>
          <h3 class="product-name">{{ product.name }}</h3>
          <p class="product-price">{{ product.price }} ₽</p>
          <button @click="openProduct(product)" class="view-button">Подробнее</button>
          <button @click="handleAddToCart(product)" class="add-to-cart-button">Добавить в корзину
          <span v-if="cartCounts[product.id]">({{ cartCounts[product.id] }})</span>
          </button>
        </div>
      </div>
    </div>

    
    <div v-if="cart.length > 0" class="cart-page">
      <h1>Корзина</h1>
      <ul class="cart-list">
        <li v-for="(product, index) in cart" :key="product.id" class="cart-item">
          <img :src="`/duhi${product.id.replace('perfume-', '')}.png`" :alt="product.name" class="cart-image" />
          <div class="cart-info">
            <p class="cart-name">{{ product.name }}</p>
            <p class="cart-price">Цена: {{ product.price }} ₽</p>
            <p class="cart-quantity">Количество:
            <button @click="updateQuantity(product, -1)">−</button>
            {{ product.quantity }}
            <button @click="updateQuantity(product, 1)">+</button>
            </p>
            <p class="cart-total">Сумма: {{ product.price * product.quantity }} ₽
            </p>
          </div>
        </li>
      </ul>
      <h2 class="cart-total-price">Общая сумма: {{ totalPrice }} ₽</h2>
      <button class="order-button" @click="placeOrder">Оформить заказ</button>
      <button class="clear-cart-button" @click="clearCart">Очистить корзину</button>
    </div>
    
    <div v-else class="cart-page">
      <p>Ваша корзина пуста.</p>
    </div>

    <button class="catalog-button" @click="goToCart">Перейти в каталог</button>

    <div v-if="showSuccessModal" class="modal">
      <div class="modal-content">
        <p>Заказ успешно оформлен! Спасибо за покупку.</p>
        <button @click="closeModals">Закрыть</button>
      </div>
    </div>

    <div v-if="showAuthModal" class="modal">
      <div class="modal-content">
        <p>Пожалуйста, зарегистрируйтесь или войдите, чтобы оформить заказ.</p>
        <button @click="closeModals">Закрыть</button>
      </div>
    </div>
  </div>
</template>

<style scoped>

.clear-cart-button {
  background-color: #e66969;
  color: black;
  width: 100%;
  padding: 12px;
  border-radius: 25px;
  margin-top: 20px;
}

.clear-cart-button:hover {
  background-color: #d3a6a6;
}
button {
  margin-top: 10px;
  padding: 10px 20px;
  font-size: 14px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
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

.product-list {
  display: flex;
  overflow-x: auto;
  gap: 20px;
  padding-bottom: 20px;
}

.product-card {
  width: 250px;
  border-radius: 8px;
  overflow: hidden;
  background-color: rgba(255, 192, 203, 0.202);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
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

.cart-page {
  text-align: center;
  padding: 20px;
}

.cart-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center; 
  margin-right: 40px;
}



.cart-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: rgba(255, 192, 203, 0.2);
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.cart-image {
  width: 100px;
  height: 100px;
  object-fit: cover;
  border-radius: 8px;
  margin-right: 15px;
}

.cart-info {
  flex: 1;
  text-align: left;
}

.cart-name {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 5px;
}

.cart-price,
.cart-quantity,
.cart-total {
  font-size: 14px;
  color: #555;
  margin: 3px 0;
}

.cart-quantity button {
  margin: 0 5px;
  padding: 4px 10px;
  font-size: 14px;
  border: none;
  border-radius: 4px;
  background-color: #f2d2d2;
  cursor: pointer;
}

.cart-quantity button:hover {
  background-color: #d3a6a6;
}

.cart-total-price {
  font-size: 20px;
  font-weight: bold;
  margin-top: 20px;
}

.order-button {
  background-color: #d3a6a6;
  color: black;
  width: 100%;
  padding: 12px;
  border-radius: 25px;
  margin-top: 20px;
  font-size: 16px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.order-button:hover {
  background-color: #9a7a7a;
}

.catalog-button {
  background-color: #f2d2d2;
  color: black;
}

.catalog-button:hover {
  background-color: #d3a6a6;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.modal-content {
  background: white;
  padding: 20px 30px;
  border-radius: 10px;
  text-align: center;
  max-width: 300px;
}

.modal-content button {
  margin-top: 15px;
  padding: 8px 16px;
  font-size: 14px;
  background-color: #d3a6a6;
  color: black;
  border-radius: 25px;
}

.modal-content button:hover {
  background-color: #9a7a7a;
}
</style>
