<script setup>
defineProps(['id','descricao','status']);
defineEmits(['editar','excluir']);
</script>

<template> 
    <li>
        {{ descricao }}
        <a href="#" @click.prevent="$emit('excluir', id)">Excluir</a>
        <a href="#" @click.prevent="$emit('editar')">Editar</a>
    </li>   
</template>

<style scoped>
</style>