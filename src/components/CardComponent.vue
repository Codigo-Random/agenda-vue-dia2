<script>

export default {
    name: 'CardComponent',
    props: {
        user: Object
    }
}
</script>

<template>
     <div class="flex flex-col items-start space-y-2 bg-white shadow p-4">
        <p>{{ user.name }}</p>
        <p>{{ user.email }}</p>
        <p>{{ user.phone }}</p>
        <div class="flex justify-between w-full space-x-2">
            <button class="bg-red-100 text-red-500 px-4 py-2 w-full text-lg hover:bg-red-200 transition-all">Eliminar</button>
            <button class="bg-yellow-500 text-black px-4 py-2 w-full text-lg hover:bg-yellow-600 transition-all">Editar</button>
        </div>
    </div>
</template>