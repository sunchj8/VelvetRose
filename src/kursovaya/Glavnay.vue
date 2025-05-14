<script setup>
import { ref } from "vue";


const slide = ref(0);
const slides = [
  "slad1.jpg",
  "slad2.jpg",
  "slad3.jpg"
];

function prevSlide() {
  slide.value = (slide.value === 0) ? slides.length - 1 : slide.value - 1;
}

function nextSlide() {
  slide.value = (slide.value === slides.length - 1) ? 0 : slide.value + 1;
}
</script>

<template>
  <h1 class="text">Velvet Rose</h1>
  <div class="about-page">
    
    <section class="intro">
      <div class="text-box">
        <p>
          Добро пожаловать в наш парфюмерный мир! Мы — интернет-магазин, который собрал лучшие ароматы со всего мира. В нашем каталоге — оригинальные духи от известных брендов и 
          нишевые композиции для настоящих ценителей. Мы верим, что аромат — это больше, чем запах. Это настроение, воспоминания и стиль. С любовью к качеству и вниманием к каждому 
          клиенту, мы помогаем найти тот самый, идеальный аромат.
        </p>
      </div>
    </section>

    <div class="image-placeholder">
      <img src='/foto1.jpg' alt="Наши флаконы">
    </div>

    <section class="onas">
      <h2>О нас</h2>
      <div class="text-box">
        <p>
          Наш магазин создан для того, чтобы сделать покупку парфюма простой, приятной и безопасной. Мы предлагаем удобную навигацию по сайту, подробные описания и качественные 
          фотографии продукции. Все ароматы на складе — оригинальные и поступают напрямую от официальных поставщиков. Мы гарантируем быструю доставку, бережную упаковку и приятные 
          бонусы для наших постоянных клиентов.
        </p>
      </div>

      <section class="slider">
        <div class="slider-container">
          <img :src="slides[slide]" alt="Slide" class="slider-image" />
        </div>
        <div class="slider-nav">
          <button class="prev" @click="prevSlide">❮</button>
          <button class="next" @click="nextSlide">❯</button>
        </div>
      </section>

      <div class="grid">
        <div class="process-step">
          <div class="text-box">
            <h3>Вдохновение ароматами</h3>
            <p>
              Каждый аромат в нашем каталоге — это история, которая пробуждает эмоции. Мы тщательно отбираем коллекции, чтобы вы могли найти отражение настроения.
            </p>
          </div>
          <div class="image-placeholder1">
            <img src='/foto11.png' alt="Ароматы" />
          </div>
        </div>

        <div class="process-step">
          <div class="text-box">
            <h3>Эстетика и стиль</h3>
            <p>
              Мы верим, что парфюм — это не только запах, но и образ жизни. Наш магазин создан с вниманием к деталям, чтобы каждый визит приносил удовольствие и вдохновлял.
            </p>
          </div>
          <div class="image-placeholder1">
            <img src='/foto22.png' alt="Стиль" />
          </div>
        </div>

        <div class="process-step">
          <div class="text-box">
            <h3>Идеи для подарков</h3>
            <p>
              Парфюмерия — идеальный способ выразить чувства. Мы предлагаем подборки ароматов для особых случаев и любимых людей, чтобы помочь вам сделать подарок со смыслом.
            </p>
          </div>
          <div class="image-placeholder1">
            <img src='/foto33.png' alt="Подарки" />
          </div>
        </div>
      </div>
    </section>

    <section class="why-us">
      <h2>Почему выбирают нас?</h2>
      <div class="text-box">
        <p>
          Оригинальная продукция — все ароматы поступают напрямую от официальных дистрибьюторов и брендов. Быстрая доставка — мы отправляем заказы в кратчайшие сроки, 
          чтобы вы могли наслаждаться ароматом без ожидания. Бережная упаковка — каждая посылка упакована с заботой, чтобы сохранить флаконы в идеальном состоянии.
        </p>
      </div>
    </section>
  </div>
</template>

<style scoped>
.text {
  color: #D3A6A6;
}

.about-page {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Roboto', sans-serif;
}

h1, h2, h3 {
  color: #000000;
  text-align: center;
}

.text-box {
  background-color: rgba(255, 192, 203, 0.202);
  padding: 20px;
  margin: 20px 0;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  text-align: justify;
}

.slider {
  position: relative;
  max-width: 100%;
  margin-bottom: 40px;
  overflow: hidden;
}

.slider-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 500px;
  overflow: hidden;
  border-radius: 12px;
}

.slider-image {
  max-height: 100%;
  max-width: 100%;
  object-fit: cover;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  transition: opacity 0.8s ease-in-out;
  opacity: 1;
}

.slider-nav {
  position: absolute;
  top: 50%;
  width: 100%;
  display: flex;
  justify-content: space-between;
  transform: translateY(-50%);
}

.prev, .next {
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  font-size: 2em;
  padding: 10px;
  cursor: pointer;
  transition: 0.3s;
}

.prev:hover, .next:hover {
  background-color: rgba(0, 0, 0, 0.7);
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
  margin-top: 20px;
}

.process-step {
  text-align: center;
}

.image-placeholder {
  height: 520px;
  width: 60px; 
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  border-radius: 20px;
  
}

.image-placeholder img {
  height: 100%;
  width: auto;
  border-radius: 20px;
  object-fit: cover;
}

.image-placeholder1 {
  height: 180px;
  width: 180px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px auto 0;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.image-placeholder1 img {
  height: 100%;
  width: 100%;
  object-fit: cover;
  border-radius: 12px;
}
</style>
