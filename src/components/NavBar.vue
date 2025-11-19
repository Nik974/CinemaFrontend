<template>
  <nav class="navbar">
    <div class="logo">🎬 BiletUZ</div>

    <div class="menu" v-if="isLoggedIn">
      <span class="username">Witaj!</span>
      <button @click="handleLogout" class="logout-btn">Wyloguj</button>
    </div>

    <div class="menu" v-else>
      <router-link to="/login" class="login-btn">Zaloguj się</router-link>
      <router-link to="/register" class="register-link">Rejestracja</router-link>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const isLoggedIn = ref(false);

onMounted(() => {
  const token = localStorage.getItem('userToken');
  isLoggedIn.value = !!token;
});

const handleLogout = () => {
  localStorage.removeItem('userToken');

  isLoggedIn.value = false;

  router.push('/');
};
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 30px;
  background-color: #2c3e50;
  color: white;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.logo {
  font-size: 24px;
  font-weight: bold;
  color: #42b983;
}

.menu {
  display: flex;
  align-items: center;
  gap: 15px;
}


.logout-btn {
  background-color: transparent;
  border: 1px solid #e74c3c;
  color: #e74c3c;
  padding: 6px 12px;
  cursor: pointer;
  border-radius: 5px;
  transition: all 0.3s;
}

.logout-btn:hover {
  background-color: #e74c3c;
  color: white;
}


.login-btn {
  background-color: #42b983;
  color: white;
  text-decoration: none;
  padding: 8px 15px;
  border-radius: 5px;
  font-weight: bold;
  transition: background-color 0.3s;
}

.login-btn:hover {
  background-color: #3aa876;
}


.register-link {
  color: #ccc;
  text-decoration: none;
  font-size: 14px;
}

.register-link:hover {
  color: white;
}
</style>
