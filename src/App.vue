<script setup>
import { ref, computed } from 'vue';
import TaskChild from './components/TaskChild.vue';

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
const tarefasFiltradas = computed(() => {
 if (usuario.value.trim() == ''){
  return tarefas.value;
 }
 else{
  return tarefas.value.filter(t => t.desc.includes(usuario.value))
 }
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
function delet(idTarefa){
   const posicao = tarefas.value.findIndex(t => t.id === idTarefa);
   tarefas.value.splice(posicao,1);
   return false
}
function edit(idTarefa){
    posicaoAlterar.value =  tarefas.value.findIndex(t => t.id === idTarefa);
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
function ordenar(){
  tarefas.value.sort((a,b) => a.desc.localeCompare(b.desc,'pt-BR'))
}

</script>

<template>

    <div class="container">
        <input type="text" v-model="novaTarefa" @keyup.enter = "add"> <button @click="add">add</button>

        <ul>
            <!-- <li v-for="item in tarefasFiltradas" :key="item.id">

                <span  @click="marcarConcluida(item.id)" :class="{concluida: item.status == 'concluida'}">
                    {{ item.desc }}
                </span>

                <span><a href="#" @click.prevent="delet(item)">Excluir</a></span>
                <span><a href="#" @click.prevent="edit(item)">Edit</a></span>
                 
            </li> -->

            <TaskChild v-for="tarefa in tarefasFiltradas" :key="tarefa.id" :id="tarefa.id" :descricao="tarefa.desc" :status="tarefa.status" @excluir="delet" @editar="edit">
                
            </TaskChild>
            
        </ul>
        <input type="text" v-model="usuario" placeholder="Filtrar...">
        <button @click.prevent="ordenar()">Ordenar</button>
        <div>

        <span>Pendentes: {{ tarefasPendentes }}</span>
        <span> Cocluidas: {{ tarefasConcluidas }}</span>

        </div>
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
