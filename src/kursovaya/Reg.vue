<script setup>
import { reactive, defineProps, defineEmits } from "vue";

const props = defineProps(["user"]);
const emit = defineEmits(["RegistorEmit", "logoutEmit"]);

const local = reactive({
  username: "",
  password: "",
  errorUsername: false,
  errorPassword: false,
});

function check() {
  let passCheck = true;
  
  if (!/^[a-zA-Z0-9_]{3,}$/.test(local.username)) {
    local.errorUsername = true;
    passCheck = false;
  } else {
    local.errorUsername = false;
  }
  
  if (local.password.length < 6) {
    local.errorPassword = true;
    passCheck = false;
  } else {
    local.errorPassword = false;
  }

  if (passCheck) {
    emit("RegistorEmit", {
      username: local.username,
      password: local.password,
    });
  }
}
</script>

<template>
  <div class="container">
    <div v-if="!props.user" class="form-box">
      <h1>Регистрация</h1>

      <label>
        Имя пользователя:
        <input type="text" v-model="local.username" placeholder="Например: user123" />
      </label>
      <div class="error" v-if="local.errorUsername">Имя должно быть минимум 3 символа и содержать только латиницу/цифры</div>

      <label>
        Пароль:
        <input type="password" v-model="local.password" placeholder="Не менее 6 символов" />
      </label>
      <div class="error" v-if="local.errorPassword">Пароль должен быть не менее 6 символов</div>

      <button class="btn" @click="check">Зарегистрироваться</button>
    </div>

    <div v-else class="profile-box">
      <h1>Личный кабинет</h1>
      <p><strong>Имя пользователя:</strong> {{ props.user.username }}</p>
      <p><strong>Пароль:</strong>********</p>
      <button class="btn logout" @click="$emit('logoutEmit')">Выйти</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 400px;
  margin: 40px auto;
  padding: 24px;
  background: rgba(255, 192, 203, 0.202);
  border-radius: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  font-family: "Segoe UI", sans-serif;
  
}


h1 {
  text-align: center;
  margin-bottom: 24px;
  font-size: 24px;
  color: #333;
}

.form-box label {
  display: block;
  margin-bottom: 12px;
  font-weight: bold;
  color: #333;
}

input[type="text"],
input[type="password"] {
  width: 100%;
  padding: 12px;
  margin-top: 6px;
  border: 1px solid #ccc;
  border-radius: 25px;
  box-sizing: border-box;
  font-size: 16px;
  transition: border-color 0.3s ease;
}

input[type="text"]:focus,
input[type="password"]:focus {
  border-color: #3478f6;
  outline: none;
}

.error {
  color: #d60000;
  font-size: 0.9em;
  margin-bottom: 12px;
}

.btn {
  width: 100%;
  padding: 12px;
  background-color: #D3A6A6;
  color: rgb(0, 0, 0);
  border: none;
  border-radius: 25px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 16px;
  transition: background 0.3s ease, transform 0.3s ease;
}

.btn:hover {
  background-color: #9A7A7A;
  transform: translateY(-2px);
}

.logout {
  background-color: #e74c3c;
}

.logout:hover {
  background-color: #c0392b;
  transform: translateY(-2px);
}

.profile-box {
  text-align: center;
}

.profile-box h1 {
  font-size: 24px;
  color: #333;
}

.profile-box p {
  font-size: 16px;
  color: #333;
  margin-bottom: 20px;
}

.profile-box .logout {
  margin-top: 20px;
}
</style>

