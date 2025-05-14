<script setup>
import { ref, reactive, computed } from "vue";
import Glavnay from "./Glavnay.vue";
import Katolog from "./Katolog.vue";
import Dostavka from "./Dostavka.vue";
import Korzina from "./Korzina.vue";
import Reg from "./Reg.vue";

const local = reactive({
  curPage: 1,
  user: null,
  cart: [],
});

function regist(data) {
  local.user = data;
  localStorage.setItem("user", JSON.stringify(data));
}

function logoutUser() {
  local.user = null;
  localStorage.removeItem("user");
}

const authButtonText = computed(() => (local.user ? local.user.username : "Войти"));

function addToCart(product) {
  const existing = local.cart.find((item) => item.id === product.id);
  if (existing) {
    existing.quantity++;
  } else {
    local.cart.push({ ...product, quantity: 1 });
  }
}

const cartGrouped = computed(() => {
  return local.cart.map((item) => ({
    id: item.id,
    name: item.name,
    price: item.price,
    quantity: item.quantity,
    totalPrice: item.price * item.quantity,
  }));
});

const totalAmount = computed(() => {
  return cartGrouped.value.reduce((sum, item) => sum + item.totalPrice, 0);
});

function openCart() {
  local.curPage = 5;
}

function openCatalog() {
  local.curPage = 2;
}

function goToGlav() {
  local.curPage = 1;
}
</script>

<template>
  <div class="app-wrapper">
    <div class="main-content">
      <div class="header">
        <div class="menu">
          <img 
            src='/logo.jpg' 
            alt="Логотип" 
            class="logo-img" 
            @click="goToGlav" 
          />
          <button @click="local.curPage = 1" :class="{ menuIt: local.curPage == 1 }">О магазине</button>
          <button @click="local.curPage = 4" :class="{ menuIt: local.curPage == 4 }">Доставка</button>
          <button @click="openCatalog" :class="{ menuIt: local.curPage == 2 }">Каталог</button>
          <button @click="openCart" :class="{ menuIt: local.curPage == 5 }">Корзина</button>
          <button @click="local.curPage = 6" :class="{ menuIt: local.curPage == 6, darkButton: local.user }">{{ authButtonText }}</button>
        </div>
      </div>

      <div class="content">
        <Glavnay v-if="local.curPage == 1" />
        <Katolog v-if="local.curPage == 2" :products="local.products" @addToCart="addToCart" @openCart="openCart" />
        <Dostavka v-if="local.curPage == 4" />
        <Korzina
          v-if="local.curPage == 5"
          :cart="local.cart"
          :user="local.user"
          @updateCart="(newCart) => local.cart = newCart"
          @openCatalog="openCatalog"
        />
        <Reg v-if="local.curPage == 6" :user="local.user" @RegistorEmit="regist" @logoutEmit="logoutUser" />
      </div>
    </div>

    <footer class="footer">
      <div class="footer-content">
        <div class="footer-navigation">
          <div class="footer-logo" @click="goToGlav">
            <img src='/logo.jpg' alt="Логотип" class="footer-logo-img" />
          </div>
          <div class="footer-links">
            <button @click="local.curPage = 1" :class="{ active: local.curPage == 1 }">О магазине</button>
            <button @click="local.curPage = 4" :class="{ active: local.curPage == 4 }">Доставка</button>
            <button @click="openCatalog" :class="{ active: local.curPage == 2 }">Каталог</button>
            <button @click="openCart" :class="{ active: local.curPage == 5 }">Корзина</button>
            <button @click="local.curPage = 6" :class="{ active: local.curPage == 6 }">{{ authButtonText }}</button>
          </div>
        </div>
        <div class="footer-info">
          <p>© 2023 Магазин парфюмерии. Все права защищены.</p>
          <div class="footer-contacts">
            <p>Телефон: +7 (123) 456-78-90</p>
            <p>Email: info@perfume-shop.ru</p>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(255, 192, 203, 0.202); 
  z-index: 1000;
  display: flex;
  justify-content: center;
  padding: 10px 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.main-content {
  flex: 1;
  padding: 100px 20px 20px; 
  min-height: calc(100vh - 220px);
}

.content {
  padding-bottom: 20px;
}

.menu {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}

.menu button {
  padding: 0px 40px;
  background-color: #f2d2d2; 
  border: 2px solid transparent;
  border-radius: 25px; 
  cursor: pointer;
  transition: 0.3s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.menu button:hover {
  background-color: #d3a6a6; 
}

.menuIt, .active {
  border-color: #00bcd4;
  background-color: #e0f7fa;
}

.logo-img {
  height: 50px;
  margin-right: 0px;
  cursor: pointer; 
}

.app-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.footer {
  background-color: rgba(255, 192, 203, 0.202);
  padding: 30px 20px 20px;
  box-shadow: 0 -2px 6px rgba(0, 0, 0, 0.1);
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
}

.footer-navigation {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.footer-logo {
  cursor: pointer;
}

.footer-logo-img {
  height: 40px;
}

.footer-links {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
}

.footer-links button {
  padding: 8px 20px;
  background-color: #f2d2d2;
  border: 2px solid transparent;
  border-radius: 25px;
  cursor: pointer;
  transition: 0.3s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.footer-links button:hover {
  background-color: #d3a6a6;
}

.footer-info {
  text-align: center;
  font-size: 14px;
  color: #555;
  margin-top: 20px;
}

.footer-contacts {
  margin-top: 10px;
  font-size: 13px;
}

@media (max-width: 768px) {
  .menu, .footer-links {
    flex-direction: column;
    gap: 8px;
  }
  
  .menu button, .footer-links button {
    width: 100%;
    padding: 8px 0;
  }
  
  .main-content {
    padding: 80px 10px 10px;
  }
  
  .footer-navigation {
    flex-direction: column;
    gap: 15px;
  }
}
</style>