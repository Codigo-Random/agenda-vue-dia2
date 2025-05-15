
<script>
import { baseUrl, apiKey } from "../shared/utils"

export default {
    data() {
        return {
            email: '',
            password: '',
            error: false
        }
    },
    methods: {
        login() {

            const url = baseUrl + "/auth/v1/token?grant_type=password"

            const requestOptions = {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "apikey": apiKey
                },
                body: JSON.stringify({
                    email: this.email,
                    password: this.password
                })
            }

             fetch(url, requestOptions)
                .then((response) => response.json())
                .then((result) => {

                    if(result.error_code){
                        this.error = true
                        return
                    }

                    const token = result.access_token
                    const userId = result.user.id

                    localStorage.setItem("token", token)
                    localStorage.setItem("userId", userId)
                    
                    // Navegacion programatica de VUE
                    this.$router.push("/dashboard")
                })
                .catch((error) => {
                    this.error = true
                });
        }   
    }
}
</script>

<template>
    <div class="max-w-7xl mx-auto bg-black text-white h-screen p-4 w-full">
        <div class="py-8">
            <p class="text-white text-lg">Inicia sesión</p>
        </div>
        <div class="bg-white text-black p-6 flex flex-col items-start space-y-2">
            <input v-model="email" id="email" class="w-full border border-slate-300 p-4 text-md" type="email" placeholder="email" name="">
            <input v-model="password" id="pass" class="w-full border border-slate-300 p-4 text-md" type="password" name="" placeholder="contraseña">
            <button @click="login" class="bg-yellow-500 text-black px-4 py-2 w-full text-lg hover:bg-yellow-600 transition-all">Entrar</button>

            <p v-if="error" class="text-red-500 p-3 bg-red-100 w-full">Error: Usuario o contraseña incorrectos</p>
        </div>
    </div>
</template>