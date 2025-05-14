<script setup>
import { ref } from 'vue';

const ff = [
  { id: 1, title: 'Сборка заказа', description: 'После оформления заказа наши специалисты собирают его и тщательно упаковывают для доставки.' },
  { id: 2, title: 'Упаковка', description: 'Мы гарантируем, что ваши духи будут хорошо упакованы для безопасности при транспортировке.' },
  { id: 3, title: 'Передача в доставку', description: 'После упаковки заказ передается в службу доставки для дальнейшей транспортировки.' },
  { id: 4, title: 'Доставка', description: 'Вы можете выбрать доставку Почтой России или курьером прямо на дом.' },
];
</script>

<template>
  <div class="delivery-process">
    <h1>Процесс доставки</h1>
      <div v-for="f in ff" :key="f.id" class="f">
        <h2>{{ f.title }}</h2>
        <p>{{ f.description }}</p>
      </div>
  </div>
</template>

<style scoped>
.delivery-process {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Arial', sans-serif;
}

h1 {
  text-align: center;
  color: #333;
  font-size: 28px;
  margin-bottom: 40px;
}

.f {
  background: rgba(255, 192, 203, 0.202);
  border-radius: 15px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  
}

h2 {
  color: #000000;
  font-size: 22px;
  margin-bottom: 10px;
}

p {
  font-size: 16px;
  color: #555;
}


</style>
