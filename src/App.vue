<script setup>
import { ref, computed } from 'vue'
const tarefas = ref([
    {id: 1, desc:'Prova Geografia', status: 'pendente'},
    {id: 2, desc:'Prova História', status: 'concluida'},
    {id: 3, desc:'Prova DevWeb', status: 'pendente'}
])
const novaTarefa = ref('');
const posicaoAlterar = ref(-1);
const usuario = ref('');

const tarefasPendentes = computed(() => {
    return tarefas.value.filter(t => t.status == 'pendente').length;
})
const tarefasConcluidas = computed(() => {
    return tarefas.value.filter(t => t.status == 'concluida').length;
})

function add(){
    if(posicaoAlterar.value == -1){
    let maiorID = Math.max(...tarefas.value.map(item => item.id));
    tarefas.value.push({id: maiorID + 1 , desc: novaTarefa.value,status: 'pendente'});
    }
    else{
        tarefas.value[posicaoAlterar.value].desc = novaTarefa.value;
        posicaoAlterar.value = -1 
    }
    novaTarefa.value = '';
}
function delet(item){
   const posicao = tarefas.value.findIndex(t => t.id === item.id);
   tarefas.value.splice(posicao,1);
   return false
}
function edit(item){
    posicaoAlterar.value =  tarefas.value.findIndex(t => t.id === item.id);
    novaTarefa.value = tarefas.value[posicaoAlterar.value].desc;
}
function marcarConcluida(id){
    const posicao = tarefas.value.findIndex(t => t.id == id);
    if(tarefas.value[posicao].status == 'pendente'){
        tarefas.value[posicao].status = 'concluida';
    }
    else{
        tarefas.value[posicao].status = 'pendente';
    }
}

</script>

<template>

    <div class="container">
        <input type="text" v-model="novaTarefa" @keyup.enter = "add"> <button @click="add">add</button>

        <ul>
            <li v-for="item in tarefas" :key="item.id">

                <span  @click="marcarConcluida(item.id)" :class="{concluida: item.status == 'concluida'}">
                    {{ item.desc }}
                </span>

                <span><a href="#" @click.prevent="delet(item)">Excluir</a></span>
                <span><a href="#" @click.prevent="edit(item)">Edit</a></span>
                 
            </li>
            
        </ul>
        <input type="text" v-model="usuario">
        <button @click=" tarefas.value.filter(t => t.desc.includes(usuario))">Filtrar</button>

        
    </div>
    <div>
        <span>Pendentes: {{ tarefasPendentes }}</span>
        <span> Cocluidas: {{ tarefasConcluidas }}</span>
    </div>
</template>

<style scoped>
li {
    cursor: pointer;
}
.concluida{
    text-decoration: line-through;
}
</style>
