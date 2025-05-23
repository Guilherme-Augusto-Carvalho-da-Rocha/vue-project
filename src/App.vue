<script setup>
  import { reactive } from 'vue'
  
  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [{
        titulo: 'Estudar React',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: true
      },
      {
        titulo: 'Comprar mouse',
        finalizada: false
      }
    ],
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const mudaFiltro = evento => estado.filtro = evento.target.value;

  const getTarefasFiltradas = () =>{
    const {filtro, tarefas} = estado;
    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return tarefas;
    }
  }

  const mudaTarefaFinalizada = (evento, tarefa) => tarefa.finalizada = evento.target.checked;

  const regristraTarefaTemp = evento => estado.tarefaTemp = evento.target.value;

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false
    }
    let tarefaExiste = estado.tarefas.some(tarefa => tarefa.titulo.toLowerCase() === tarefaNova.titulo.toLowerCase())
    if (tarefaExiste) {
      alert('esta tarefa ja foi registrada');
      return;
    }
    estado.tarefas.push(tarefaNova);
  }
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Voce possui {{getTarefasPendentes().length}} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input @change="regristraTarefaTemp" required type="text" placeholder="Digite aqui a descricao da tarefa" class="form-control">
        </div>
        <div class="col-md-1">
          <button class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="mudaFiltro" class="form-control">
            <option value="todas">todas</option>
            <option value="pendentes">pendentes</option>
            <option value="finalizadas">finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => mudaTarefaFinalizada(evento, tarefa)" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox" class="form-check-input">
        <label :class="{done: tarefa.finalizada}" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done{
  text-decoration: line-through;
}
</style>
