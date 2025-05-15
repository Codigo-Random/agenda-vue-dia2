<script>
import { checkAuth, baseUrl, apiKey, userId, token } from '../shared/utils'

export default {
    data() {
        return {
            name: '',
            phone: '',
            email: ''
        }
    },
    methods: {
        saveUser() {

            let url = baseUrl + "/rest/v1/contacts"

            const requestOptions = {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "apiKey": apiKey,
                    "Authorization": `Bearer ${token}`
                },
                body: JSON.stringify({
                    name: this.name,
                    phone: this.phone,
                    email: this.email,
                    user_id: userId
                })
            }

            fetch(url, requestOptions)
            .then((response) => {
                if(response.status == 201 || response.status == 204) {
                    this.$router.push("/dashboard")
                }
            })
            .catch((error) => {
                console.log(error)
            })
        }
    },
    mounted() {
        checkAuth(this.$router)
    }
}
</script>

<template>
    <div>
        <div class="max-w-xl mx-auto py-8 mt-4 bg-white">
            <h1 id="title" class="text-center text-lg">Crea un contacto nuevo</h1>
            <div class="p-8 flex flex-col space-y-3">
                <input v-model="name" class="border px-4 py-2 border-slate-400 w-full" type="text" name="" id="name" placeholder="Nombre">
                <input v-model="phone" class="border px-4 py-2 border-slate-400 w-full" type="tel" name="" id="phone" placeholder="Teléfono">
                <input v-model="email" class="border px-4 py-2 border-slate-400 w-full" type="email" name="" id="email" placeholder="Email">

                <button @click="saveUser" id="save" class="bg-blue-600 text-white px-4 py-2 w-full text-lg hover:bg-blue-900 transition-all">Guardar</button>
            </div>
        </div>
    </div>
</template>