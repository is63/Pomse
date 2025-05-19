<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

axios.defaults.baseURL = 'http://localhost:8080/api/';
const friends = ref([])
const loadingFriends = ref(true)
const noFriendsMsg = ref('')

onMounted(async () => {
  const token = sessionStorage.getItem('token')
  if (!token) {
    noFriendsMsg.value = 'Descubre a gente nueva'
    loadingFriends.value = false
    return
  }
  try {
    const response = await axios.get('friendshipsOfUser', {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    })
    // La API devuelve un array de objetos con amigo_id y accepted
    const data = response.data
    if (Array.isArray(data) && data.length > 0) {
      // Solo mostrar amigos aceptados (accepted == 1)
      const accepted = data.filter(f => String(f.accepted) === '1').slice(0, 5)
      if (accepted.length > 0) {
        friends.value = accepted.map(f => `Amigo #${f.amigo_id}`);
      } else {
        noFriendsMsg.value = 'Descubre a gente nueva'
      }
    } else {
      noFriendsMsg.value = 'Descubre a gente nueva'
    }
  } catch (e) {
    noFriendsMsg.value = 'Descubre a gente nueva'
  } finally {
    loadingFriends.value = false
  }
})

window.addEventListener('token-changed', () => {
  friends.value = []
  noFriendsMsg.value = 'Descubre a gente nueva'
  loadingFriends.value = false
})
</script>

<template>
  <aside class="friends-list-aside" aria-label="Tus amigos">
    <div class="friends-list-header">
      <h2 class="friends-list-title">Tus amigos</h2>
    </div>
    <ul v-if="!loadingFriends && friends.length" class="friends-list-ul">
      <li v-for="(friend, index) in friends" :key="index" class="friends-list-li">
        <a class="friend-link" href="#">
          <span class="friend-avatar">
            <span class="avatar-img">
              <img src="/public/icons/comment.svg" alt="avatar" width="32" height="32" />
            </span>
          </span>
          <span class="friend-info">
            <span class="friend-name">{{ friend }}</span>
            <span class="friend-meta">Amistad aceptada</span>
          </span>
        </a>
      </li>
    </ul>
    <div v-else-if="!loadingFriends && noFriendsMsg" class="no-friends-msg">{{ noFriendsMsg }}</div>
  </aside>
</template>

<style scoped>
.friends-list-aside {
  background: #232323;
  border-radius: 8px;
  margin-bottom: 1.2rem;
  padding-bottom: 0.5rem;
  padding-top: 0.7rem;
  position: fixed;
  right: 2vw;
  top: 50%;
  transform: translateY(-50%);
  min-width: 240px;
  max-width: 320px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.22);
  border: 1.5px solid rgba(255, 255, 255, 0.08);
  color: #ffd91c;
  z-index: 10;
  backdrop-filter: blur(4px);
}

.friends-list-header {
  padding-left: 1.2rem;
  padding-right: 1.2rem;
  color: #bfa600;
}

.friends-list-title {
  text-transform: uppercase;
  font-size: 0.95rem;
  font-weight: 600;
  margin: 0 0 0.5rem 0;
  letter-spacing: 0.04em;
}

.friends-list-ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.friends-list-li {
  position: relative;
  margin-top: 0;
  list-style: none;
}

.friend-link {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 0.7rem;
  padding: 0.5rem 1.2rem;
  color: #ffd91c;
  text-decoration: none;
  border-radius: 6px;
  transition: background 0.18s, color 0.18s;
  cursor: pointer;
}

.friend-link:hover {
  background: #282828;
  color: #fffbe6;
}

.friend-avatar {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 2.2rem;
  width: 2.2rem;
  min-width: 2.2rem;
  min-height: 2.2rem;
}

.avatar-img {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
  border-radius: 50%;
  background: #161515;
  overflow: hidden;
}

.avatar-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
}

.friend-info {
  display: flex;
  flex-direction: column;
  min-width: 0;
  flex: 1;
  justify-content: center;
}

.friend-name {
  font-size: 1.08rem;
  color: #ffd91c;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.friend-meta {
  font-size: 0.85rem;
  color: #bfa600;
  opacity: 0.7;
  margin-top: 0.1em;
}

.no-friends-msg {
  color: #fffbe6;
  font-size: 1em;
  text-align: center;
  padding: 0.7em 0.2em;
  padding-left: 1.2rem;
  padding-right: 1.2rem;
}
</style>