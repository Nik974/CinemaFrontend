<template>
  <div class="register-container">
    <h2>Rejestracja</h2> <form @submit.prevent="handleRegister">

    <div class="form-group">
      <label for="firstName">Imię</label>
      <input
        type="text"
        id="firstName"
        v-model="form.firstName"
        placeholder="Wpisz swoje imię"
        required
      >
    </div>

    <div class="form-group">
      <label for="lastName">Nazwisko</label>
      <input
        type="text"
        id="lastName"
        v-model="form.lastName"
        placeholder="Wpisz swoje nazwisko"
        required
      >
    </div>

    <div class="form-group">
      <label for="username">Nazwa użytkownika</label>
      <input
        type="text"
        id="username"
        v-model="form.username"
        placeholder="Wybierz login"
        required
      >
    </div>

    <div class="form-group">
      <label for="email">Email</label>
      <input
        type="email"
        id="email"
        v-model="form.email"
        placeholder="twoj@email.com"
        required
      >
    </div>

    <div class="form-group">
      <label for="password">Hasło</label>
      <input
        type="password"
        id="password"
        v-model="form.password"
        placeholder="Hasło"
        required
      >
    </div>

    <button type="submit" :disabled="loading">
      {{ loading ? 'Rejestracja...' : 'Zarejestruj się' }}
    </button>

    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </form>

    <div class="login-link">
      <p>Masz już konto? <router-link to="/login">Zaloguj się</router-link></p>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const router = useRouter();
const loading = ref(false);
const errorMessage = ref('');


const form = reactive({
  firstName: '',
  lastName: '',
  username: '',
  email: '',
  password: ''
});

const handleRegister = async () => {
  loading.value = true;
  errorMessage.value = '';

  try {

    const response = await axios.post('http://localhost:8080/api/auth/register', {
      first_name: form.firstName,
      last_name: form.lastName,

      username: form.username,
      email: form.email,
      password: form.password
    });

    console.log('Success:', response.data);
    alert('Rejestracja zakończona sukcesem! Zaloguj się.');


    router.push('/login');

  } catch (error) {
    console.error('Registration error:', error);
    if (error.response && error.response.data && error.response.data.message) {
      errorMessage.value = `Błąd: ${error.response.data.message}`;
    } else {
      errorMessage.value = 'Wystąpił błąd podczas rejestracji. Spróbuj ponownie.';
    }
  } finally {
    loading.value = false;
  }
};
</script>

<style scoped>
.register-container {
  max-width: 500px;
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

.login-link {
  text-align: center;
  margin-top: 20px;
  font-size: 14px;
}

.login-link a {
  color: #42b983;
  text-decoration: none;
}
</style>
