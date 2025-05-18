<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'

const expanded = ref(false)
</script>

<template>
    <div class="sidebar" :class="{ expanded }" @mouseenter="expanded = true" @mouseleave="expanded = false">
        <div class="sidebar-content">
            <RouterLink class="menu-item text-decoration-none text-white fw-semibold" to="/">
                <span class="icon">🏠</span>
                <span class="text">Inicio</span>
            </RouterLink>
            <RouterLink class="menu-item text-decoration-none text-white fw-semibold" to="/documentos">
                <span class="icon">📄</span>
                <span class="text">Documentos</span>
            </RouterLink>
            <RouterLink class="menu-item text-decoration-none text-white fw-semibold" to="/configuracion">
                <span class="icon">⚙️</span>
                <span class="text">Configuración</span>
            </RouterLink>
        </div>
    </div>
</template>

<style scoped>
.sidebar {
    position: fixed;
    top: 7vh;
    left: 0;
    height: 90vh;;
    width: 2.5vw;
    min-width: 48px;
    max-width: 220px;
    background: #1b1a1a;
    color: #ffd91c;
    transition: width 0.2s;
    overflow-x: hidden;
    z-index: 1000;
    box-shadow: 0.2vw 0 0.7vw rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 2vh;
    border-radius: 1.2vw;
    border: 1.5px solid #575757;
}

.sidebar.expanded {
    width: 10vw;
    min-width: 120px;
}

@media (max-width: 900px) {
    .sidebar {
        width: 8vw;
        min-width: 40px;
    }

    .sidebar.expanded {
        width: 40vw;
        min-width: 120px;
    }
}

@media (max-width: 600px) {
    .sidebar {
        position: fixed;
        top: 7vh;
        left: 0;
        width: 0;
        min-width: 0;
        height: 0;
        border: none;
        box-shadow: none;
        background: transparent;
        pointer-events: none;
    }

    .sidebar.expanded {
        width: 60vw;
        min-width: 120px;
        height: calc(100vh - 7vh);
        background: #222;
        pointer-events: auto;
        box-shadow: 0.2vw 0 0.7vw rgba(0, 0, 0, 0.1);
    }
}

.sidebar.expanded .menu-item {
    justify-content: flex-start;
    width: 100%;
}

.sidebar-content {
    width: 100%;
    margin-top: 2vh;
    display: flex;
    flex-direction: column;
    gap: 2vh;
}

.menu-item {
    padding: 1.2vh;
    border-radius: 0.8vw;
    cursor: pointer;
    white-space: nowrap;
    transition: background 0.2s;
    display: flex;
    align-items: center;
    justify-content: center;
}

.menu-item .icon {
    font-size: 2.2vh;
}

.menu-item .text {
    margin-left: 1vw;
    transition: opacity 0.2s, width 0.2s;
    white-space: nowrap;
    overflow: hidden;
}

.sidebar:not(.expanded) .menu-item .text {
    opacity: 0;
    width: 0;
    margin-left: 0;
    pointer-events: none;
}

.sidebar.expanded .menu-item {
    justify-content: flex-start;
}

.menu-item:hover {
    background: rgba(255, 221, 28, 0.1);
}
</style>
