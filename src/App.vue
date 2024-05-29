<script setup>
import { reactive } from 'vue';
  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefa: [
    {
      titulo: 'Estudar ES6',
      finalizada: false
    },
    {
      titulo: 'Estudar Javascript',
      finalizada: false
    },
    {
      titulo: 'Estudar SASS',
      finalizada: true
    },
    {
      titulo: 'Ir para academia',
      finalizada: true
    }
  ]
})

const getTarefasPendentes = () => {
 return estado.tarefa.filter(tarefa => !tarefa.finalizada)
}
const getTarefasFinalizadas = () => {
 return estado.tarefa.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  //desestruturação
  const {filtro} = estado;

  switch (filtro){
    case 'pendentes': 
      return getTarefasPendentes();
    case 'finalizadas': 
      return getTarefasFinalizadas;
      default:
        return estado.tarefa
  }
}
const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefa.push(tarefaNova)
  estado.tarefaTemp = ''
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
    <h1>Minhas Tarefas</h1>
    <p>
      Você possui {{ getTarefasPendentes().length }} tarefas
    </p>
  </header>
  <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite o nome da taerefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="evento => estado.filtro = evento.target.value" class="form-control">
          <option value="todas">Todas Tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>

  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">{{ tarefa.titulo }}</label>
    </li>
  </ul>
</div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
