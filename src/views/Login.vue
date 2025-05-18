<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

const username = ref('')
const password = ref('')
const error = ref('')
const loading = ref(false)
const router = useRouter()

async function login() {
  error.value = ''
  loading.value = true
  try {
    const response = await axios.post('http://localhost:8080/api/login', {
      username: username.value,
      password: password.value
    })
    if (response.data && response.data.token) {
      sessionStorage.setItem('token', response.data.token)
      router.push('/')
    } else {
      error.value = 'Credenciales incorrectas.'
    }
  } catch (e) {
    error.value = 'Usuario o contraseña incorrectos.'
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="login-bg">
    <form class="login-card" @submit.prevent="login">
      <h2 class="login-title">Iniciar Sesión</h2>
      <div v-if="error" class="login-error">{{ error }}</div>
      <div class="login-group">
        <label for="username">Usuario</label>
        <input id="username" v-model="username" type="text" required autocomplete="username" />
      </div>
      <div class="login-group">
        <label for="password">Contraseña</label>
        <input id="password" v-model="password" type="password" required autocomplete="current-password" />
      </div>
      <button class="login-btn" :disabled="loading">
        {{ loading ? 'Accediendo...' : 'Acceder' }}
      </button>
    </form>
  </div>
</template>

<style scoped>
.login-bg {
  min-height: 75vh;
  background: #161515;
  display: flex;
  align-items: center;
  justify-content: center;
}
.login-card {
  background: #222;
  color: white;;
  border-radius: 1vw;
  box-shadow: 0 2px 16px rgba(0,0,0,0.25);
  padding: 3vh 3vw 2vh 3vw;
  min-width: 320px;
  max-width: 90vw;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.login-title {
  margin-bottom: 2vh;
  font-size: 2rem;
  font-weight: bold;
  color: white;
}
.login-group {
  width: 100%;
  margin-bottom: 2vh;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.login-group label {
  color: white;
  margin-bottom: 0.5vh;
  font-weight: 500;
}
.login-group input {
  width: 100%;
  padding: 0.7em 1em;
  border-radius: 0.5em;
  border: 1px solid #575757;
  background: #161515;
  color: white;
  font-size: 1.1rem;
  outline: none;
  transition: border 0.2s;
}
.login-group input:focus {
  border: 1.5px solid #ffd91c;
}
.login-btn {
  width: 100%;
  padding: 0.8em 0;
  background: linear-gradient(90deg, #ffd91c 60%, #bfa600 100%);
  color: #222;
  font-weight: bold;
  font-size: 1.1rem;
  border: none;
  border-radius: 0.5em;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
  margin-top: 1vh;
}
.login-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}
.login-error {
  color: #ff4d4f;
  background: #2b1a1a;
  border-radius: 0.5em;
  padding: 0.5em 1em;
  margin-bottom: 1vh;
  width: 100%;
  text-align: center;
  font-size: 1rem;
}
</style>
