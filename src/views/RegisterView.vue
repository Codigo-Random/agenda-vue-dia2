<script>
import { baseUrl, apiKey } from "../shared/utils"

export default {
    data() {
        return {
            email: '',
            password: '',
            error: null
        }
    },
    methods: {
        register() {

            const url = baseUrl + "/auth/v1/signup"

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
                        
                        if(result.error_code === "user_already_exists"){
                            this.error = "El usuario ya existe"
                        } else if(result.error_code === "weak_password"){
                            this.error = "La contraseña es debil"
                        } else {
                            this.error = "Error al crear el usuario"
                        }

                        return
                    }

                    const token = result.access_token
                    const userId = result.user.id

                    localStorage.setItem("token", token)
                    localStorage.setItem("userId", userId)
                    
                    // Navegacion programatica de VUE
                    this.$router.push("/dashboard")
                })
                .catch((error) => console.error(error));
        }   
    }
}
</script>


<template>
    <div class="bg-black text-white h-screen p-4 w-full">
        <div class="py-8">
            <p class="text-white text-lg">Crea una cuenta</p>
        </div>
        <div class="bg-white text-black p-6 flex flex-col items-start space-y-2">
            <input v-model="email" id="email" class="w-full border border-slate-300 p-4 text-md" type="email" placeholder="email" name="">
            <input v-model="password" id="pass" class="w-full border border-slate-300 p-4 text-md" type="password" name="" placeholder="contraseña">
            <button @click="register" class="bg-yellow-500 text-black px-4 py-2 w-full text-lg hover:bg-yellow-600 transition-all">Registrarse</button>

            <p v-if="error" class="text-red-500 p-3 bg-red-100 w-full">Error: {{ error }}</p>

        </div>
    </div>
</template>