<script>
import { RouterLink, RouterView } from 'vue-router'

export default {
    name: 'App',
    components: {
        RouterLink,
        RouterView
    },
    data() {
      return {
        isLogged: localStorage.getItem("token") ? true : false
      }
    },
    watch: {
      $route() {
        this.isLogged = localStorage.getItem("token") ? true : false
      }
    },
    methods: {
        logout() {
            localStorage.removeItem("token")
            localStorage.removeItem("userId")
            this.isLogged = false
            this.$router.push("/login")
        }
    }
}
</script>

<template>
  <header>

    <div class="max-w-7xl mx-auto py-4 px-4 sm:px-6 lg:px-8 bg-slate-200 border-b border-slate-400 shadow">
      <nav class="flex space-x-4">
        <RouterLink class="hover:bg-yellow-500 transition-all px-2 py-1" to="/">Home</RouterLink>
        <RouterLink v-if="!isLogged" class="hover:bg-yellow-500 transition-all px-2 py-1" to="/login">Login</RouterLink>
        <RouterLink v-if="!isLogged" class="hover:bg-yellow-500 transition-all px-2 py-1" to="/register">Register</RouterLink>
        <RouterLink v-if="isLogged" class="hover:bg-yellow-500 transition-all px-2 py-1" to="/dashboard">Dashboard</RouterLink>
        <RouterLink v-if="isLogged" class="hover:bg-yellow-500 transition-all px-2 py-1" to="/new-user">New user</RouterLink>

        <button v-if="isLogged" class="hover:bg-yellow-500 transition-all px-2 py-1" @click="logout">Logout</button>
      </nav>
    </div>

  </header>

  <RouterView />
</template>
