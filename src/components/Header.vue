<script setup>
import { ref } from 'vue'

const logoSrc = ref('/public/favicon2.png')
const hasToken = ref(!!sessionStorage.getItem('token'))

function onLogoEnter() {
    logoSrc.value = '/public/favicon3.png'
}
function onLogoLeave() {
    logoSrc.value = '/public/favicon2.png'
}

function updateTokenStatus() {
    hasToken.value = !!sessionStorage.getItem('token')
}

window.addEventListener('storage', updateTokenStatus)
window.addEventListener('token-changed', updateTokenStatus)

function logout() {
    sessionStorage.removeItem('token')
    hasToken.value = false
    window.dispatchEvent(new Event('token-changed'))
    // Redirige o recarga si lo necesitas
}
</script>

<template>
    <nav class="navbar navbar-expand-md navbar-dark">
        <div class="container-fluid container pb-3">
            <!-- Parte Izquierda -->
            <a class="navbar-brand d-flex align-items-center ml-5" href="/" @mouseenter="onLogoEnter"
                @mouseleave="onLogoLeave">
                <img :src="logoSrc" width="32" alt="Logo" />
                <span class="fw-semibold fs-4">POMSE</span>
            </a>
            <!-- Parte Derecha -->
            <div class="collapse navbar-collapse justify-content-end">
                <ul class="navbar-nav mb-md-0">
                    <li class="nav-item">
                        <a class="btn btn-outline-success me-2" href="/">Home</a>
                    </li>
                    <li class="nav-item" v-if="hasToken">
                        <form @submit.prevent="logout" class="d-inline">
                            <button type="submit" class="btn btn-outline-danger">Log out</button>
                        </form>
                    </li>
                    <li class="nav-item" v-else>
                        <a class="btn btn-outline-primary" href="/login">Iniciar sesión</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<style scoped>
.container {
    max-width: 190vh;
}

.navbar {
    background: linear-gradient(to bottom, #222 73%, black 80%, transparent 93%);
    height: 9vh;
    min-height: 48px;
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    z-index: 1050;
}

@media (max-width: 600px) {
    .navbar {
        height: 6vh;
        min-height: 40px;
        padding: 0 1vw;
    }

    .navbar-brand span {
        font-size: 1.1rem;
    }

    .navbar-brand img {
        width: 24px;
    }
}

.navbar .navbar-brand,
.navbar .nav-link,
.navbar span {
    color: rgb(255, 221, 28) !important;
}
</style>
