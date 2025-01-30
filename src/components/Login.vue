<template>
    <div class="auth-container">
      <div class="auth-box">
        <h2 class="auth-title">Login</h2>
        <form @submit.prevent="handleSubmit">
          <div class="input-group">
            <label for="login">Login</label>
            <input type="text" v-model="login" id="login" placeholder="Enter your login" required />
          </div>
          <div class="input-group">
            <label for="password">Password</label>
            <input type="password" v-model="password" id="password" placeholder="Enter your password" required />
          </div>
          <button type="submit" class="btn-submit">Login</button>
          <p class="switch-auth">
            Don't have an account? <a href="#" @click="toggleForm">Register</a>
          </p>
        </form>
      </div>
    </div>
</template>
  
<script setup>
import { ref, defineEmits } from 'vue';
import axios from 'axios';
  
const login = ref('');
const password = ref('');
const emit = defineEmits(['toggle-form', 'login-success']);
  
const handleSubmit = async () => {
  try {
    await axios.post('http://127.0.0.1:8000/api/login', {
      login: login.value,
      password: password.value,
    });
    emit('login-success', login.value);
  } catch (error) {
    console.error('Login Failed:', error.response?.data?.message || error.message);
  }
};

const toggleForm = () => {
  emit('toggle-form', 'register'); // Сообщаем родителю, что нужно переключиться на форму регистрации
};
</script>
  
  <style scoped>
  .auth-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
  }
  
  .auth-box {
    background: #1e1e1e;
    padding: 40px;
    border-radius: 10px;
    width: 100%;
    max-width: 400px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    animation: slideIn 0.5s ease-out;
  }
  
  .auth-title {
    color: #fff;
    text-align: center;
    margin-bottom: 30px;
    font-size: 24px;
  }
  
  .input-group {
    margin-bottom: 20px;
  }
  
  .input-group label {
    color: #bbb;
    display: block;
    margin-bottom: 8px;
  }
  
  .input-group input {
    width: 100%;
    padding: 10px;
    background: #333;
    border: 1px solid #555;
    color: #fff;
    border-radius: 5px;
  }
  
  .input-group input:focus {
    outline: none;
    border-color: #4e90fe;
  }
  
  .btn-submit {
    width: 100%;
    padding: 12px;
    background: #4e90fe;
    border: none;
    color: #fff;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
  }
  
  .switch-auth {
    text-align: center;
    margin-top: 20px;
  }
  
  .switch-auth a {
    color: #4e90fe;
    text-decoration: none;
  }
  
  .switch-auth a:hover {
    text-decoration: underline;
  }
  
  @keyframes slideIn {
    0% {
      transform: translateY(0);
      opacity: 0;
    }
    100% {
      transform: translateY(0);
      opacity: 1;
    }
  }
  </style>
  