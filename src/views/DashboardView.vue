<script>
import CardComponent from '../components/CardComponent.vue'
import { checkAuth, baseUrl, apiKey, userId, token } from "../shared/utils"
import { RouterLink } from 'vue-router'

export default {
    components: {
        CardComponent,
        RouterLink
    },
    data() {
        return {
            users: []
        }
    },
    methods: {
        loadContacts() {
            let url = baseUrl + "/rest/v1/contacts"
            url += `?user_id=eq.${userId}`


            const requestOptions = {
                    method: "GET",
                    headers: {
                        "apiKey": apiKey,
                        "Authorization": `Bearer ${token}`
                    }
                }

            console.log("URL: ", requestOptions)

            fetch(url, requestOptions)
                    .then((res) => res.json())
                    .then((data) => {
                        console.log("La API ha respondido: ", data)
                        this.users = data
                    })
                    .catch((error) => {
                        console.log("Ha ocurrido un error: ", error)
                    })
        }
    },
    mounted() {
        checkAuth(this.$router)
        this.loadContacts()
    }
}
</script>

<template>
    <div class="max-w-7xl mx-auto bg-slate-100 h-screen p-4 w-full">
        <h1 class="text-2xl font-bold">Mi agenda</h1>
        <RouterLink to="/new-user" class="bg-yellow-500 text-black px-4 py-2 w-full text-lg hover:bg-yellow-600 transition-all">Nuevo contacto</RouterLink>

        <div class="grid grid-cols-3 mt-6 gap-4">
            <CardComponent v-for="user in users" :key="user.id" :user="user" />
        </div>
    </div>
</template>