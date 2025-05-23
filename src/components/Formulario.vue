<script setup>
    const props = defineProps(['cadastraTarefa','registraTarefaTemp', 'mudaFiltro'])
</script>

<template>
    <form @submit.prevent="cadastraTarefa">
        <div class="row">
            <div class="col">
                <input @change="registraTarefaTemp" required type="text" placeholder="Digite aqui a descricao da tarefa" class="form-control">
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

</template>