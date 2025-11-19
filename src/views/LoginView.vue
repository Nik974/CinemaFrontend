<template>
  <div class="login-container">
    <h2>Logowanie</h2> <form @submit.prevent="handleLogin">

    <div class="form-group">
      <label for="username">Nazwa użytkownika</label>
      <input
        type="text"
        id="username"
        v-model="username"
        placeholder="Wpisz login"
        required
      >
    </div>

    <div class="form-group">
      <label for="password">Hasło</label>
      <input
        type="password"
        id="password"
        v-model="password"
        placeholder="Wpisz hasło"
        required
      >
    </div>

    <button type="submit" :disabled="loading">
      {{ loading ? 'Logowanie...' : 'Zaloguj się' }}
    </button>

    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </form>

    <div class="register-link">
      <p>Nie masz konta? <router-link to="/register">Zarejestruj się</router-link></p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const router = useRouter();


const username = ref('');
const password = ref('');

const loading = ref(false);
const errorMessage = ref('');

const handleLogin = async () => {
  loading.value = true;
  errorMessage.value = '';

  try {

    const response = await axios.post('http://localhost:8080/api/auth/login', {
      username: username.value,
      password: password.value
    });

    console.log('Zalogowano pomyślnie:', response.data);



    if (response.data.token) {
      localStorage.setItem('userToken', response.data.token);
    }


    router.push('/');

  } catch (error) {
    console.error('Błąd logowania:', error);

    if (error.response && (error.response.status === 401 || error.response.status === 403)) {
      errorMessage.value = 'Nieprawidłowa nazwa użytkownika lub hasło.';
    } else {
      errorMessage.value = 'Wystąpił błąd serwera. Spróbuj ponownie później.';
    }
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>

.login-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 30px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #555;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 12px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3aa876;
}

button:disabled {
  background-color: #a0dcb6;
  cursor: not-allowed;
}

.error {
  color: #e74c3c;
  text-align: center;
  margin-top: 15px;
}

.register-link {
  text-align: center;
  margin-top: 20px;
  font-size: 14px;
}

.register-link a {
  color: #42b983;
  text-decoration: none;
}
</style>
