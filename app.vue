<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { UseLoginStore } from './stores/login';
const headerIsVisible = ref(true)

const router = useRouter()

async function SignOut() {
  await apiUsersLogout()
  await loginStore.fetch()
}
async function SignIn() {
  router.push('/login')
}
async function SignUp() {
  router.push('/register')
}

async function connect_current_match() {
  router.push('/match/' + loginStore.matchId)
}

const loginStore = UseLoginStore()
await callOnce(loginStore.fetch)

onMounted(async () => {
  await loginStore.fetch()
})

</script>

<template>
  <div class="header" v-if="headerIsVisible">
    <div class="left_menu">
      <div class="link">
        <a href="/">HOME</a>
      </div>
      <div class="link">
        <a href="/lobby">Lobby</a>
      </div>
    </div>
    <div class="right_menu">
      <input class="login_button connect_to_current" type="button" value="Current Match" @click="connect_current_match"
        v-if="loginStore.InCurrentMatch" />
      <div class="profile" v-if="loginStore.auth">
        <input class="login_button" value="Sign Out" type="button" @click="SignOut">
        <p class="login">{{ loginStore.username }}</p>
      </div>
      <div class="profile" v-else>
        <input class="login_button" value="Sign In" type="button" @click="SignIn">
        <input class="login_button" value="Sign Up" type="button" @click="SignUp">
      </div>
    </div>
  </div>
  <div class="main">
    <NuxtPage />
  </div>


</template>

<style>
#__nuxt {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

body {
  background-color: #222020;
  color: white;
  height: 100vh;
}

.main {
  width: 100%;
  height: 100%;
}

* {
  padding: 0;
  margin: 0;
}

::-webkit-scrollbar {
  background-color: #111010;
}

::-webkit-scrollbar-thumb {
  background-color: #222020;
  border-radius: 6px;
  outline: 2px solid #111010;
  border: none;
}

::-webkit-scrollbar-track {
  background-color: #111010;
}
</style>

<style scoped>
.header {
  height: 7%;
  width: 100%;
  background-color: #111010;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.header>* {
  caret-color: transparent;
  user-select: none;
}

.left_menu {
  display: flex;
  flex-direction: row;
  width: max-content;
  height: 100%;
}

.right_menu {
  display: flex;
  flex-direction: row;
  width: max-content;
  height: 100%;
}

.link {
  width: 100%;
  height: 100%;
  justify-content: center;
  align-content: center;
}

.link a {
  
  text-decoration: none;
  color: white;
  padding: 15px;
  font-size: 25px;
}

.link:hover {
  background-color: #222020;
}

.login_button:hover {
  background-color: #222020;
}

.profile {
  display: flex;
  flex-direction: row;
  width: 100%;
}

.login_button {
  height: 100%;
  background-color: #111010;
  color: white;
  border: none;
  padding: 15px;
  font-size: 25px;
}

.login_button:hover {
  background-color: #222020;
}

.connect_to_current {
  background-color: #550066;
}

.connect_to_current:hover {
  background-color: #770099;
}

.login {
  font-size: 30px;
  padding: 15px;
}
</style>
