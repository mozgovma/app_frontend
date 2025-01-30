<template>
  <div id="app">

    <!-- Чат и панель пользователя -->
    <div v-if="isAuthenticated" class="chat-container">
      <div class="user-panel">
        <span class="user-login">{{ userLogin }}</span>
        <button class="logout-btn" @click="handleLogout">Logout</button>
      </div>
      <Chat />
    </div>

    <!-- Авторизация и регистрация -->
    <div v-if="!isAuthenticated && !authHidden">
      <Login v-if="isLoginForm" @toggle-form="switchForm" @login-success="handleAuthSuccess" />
      <Register v-else @toggle-form="switchForm" @register-success="handleAuthSuccess" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Login from './components/Login.vue';
import Register from './components/Register.vue';
import Chat from './components/Chat.vue';

const isLoginForm = ref(true); // Состояние для переключения форм
const isAuthenticated = ref(false);
const authHidden = ref(false);
const userLogin = ref('');

onMounted(() => {
  const user = localStorage.getItem('user');
  if (user) {
    userLogin.value = user;
    isAuthenticated.value = true;
  }
});

// Переименовали функцию с toggleForm на switchForm, чтобы избежать конфликта
const switchForm = (form) => {
  isLoginForm.value = form === 'login';
};

const handleAuthSuccess = (login) => {
  if (login) {
    localStorage.setItem('user', login);
    userLogin.value = login;
  }
  isAuthenticated.value = true;
};

const handleLogout = () => {
  localStorage.removeItem('user');
  isAuthenticated.value = false;
  userLogin.value = '';
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #121212;
  overflow: hidden;
}

/* Контейнер чата */
.chat-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Панель пользователя */
.user-panel {
  position: absolute;
  top: 10px;
  right: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.user-login {
  color: #fff;
  font-size: 1rem;
  font-weight: bold;
}

.logout-btn {
  background: #ff4e4e;
  color: #fff;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
}

.logout-btn:hover {
  background: #ff2e2e;
}
</style>
