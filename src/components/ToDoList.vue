<script setup>
    const props = defineProps(['mudaTarefaFinalizada', 'getTarefasFiltradas', 'getTarefasPendentes', `getTarefasFinalizadas`, 'valorFiltro'])
</script>

<template>
    <span v-if="props.getTarefasPendentes().length <= 0 && valorFiltro == 'pendentes'">Nao existem tarefas pendentes</span>
    <span v-if="props.getTarefasFinalizadas().length <= 0 && valorFiltro == 'finalizadas'">Nao existem tarefas finalizadas</span>
    <ul v-else class="list-group mt-4">
        <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
            <input @change="evento => props.mudaTarefaFinalizada(evento, tarefa)" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox" class="form-check-input">
            <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
                {{ tarefa.titulo }}
            </label>
        </li>
    </ul>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>