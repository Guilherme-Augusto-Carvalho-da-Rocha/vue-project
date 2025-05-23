<script setup>
  import { reactive } from 'vue'
  import Cabecalho from './components/Cabecalho.vue'
  import Formulario from './components/Formulario.vue'
  import ToDoList from './components/ToDoList.vue'
  
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

  const registraTarefaTemp = evento => estado.tarefaTemp = evento.target.value;

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
    <!-- Header -->
      <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <!-- Form -->
      <Formulario :cadastra-tarefa="cadastraTarefa" :muda-filtro="mudaFiltro" :registra-tarefa-temp="registraTarefaTemp" />
    <!-- ToDoList -->
      <ToDoList :get-tarefas-filtradas="getTarefasFiltradas" :muda-tarefa-finalizada="mudaTarefaFinalizada" :get-tarefas-pendentes="getTarefasPendentes" :get-tarefas-finalizadas="getTarefasFinalizadas" :valor-filtro="estado.filtro"/>
  </div>
</template>