<script setup>
import { reactive, computed } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
  ],
});

const getTarefasPendentes = () => {
  return estado.tarefas.filter((tarefa) => !tarefa.finalizada);
};

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter((tarefa) => tarefa.finalizada);
};

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
};

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  };
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
};

const todasTarefasFinalizadas = computed(() => {
  return estado.tarefas.every((tarefa) => tarefa.finalizada);
});
</script>


<template class="body">
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <div v-if="todasTarefasFinalizadas">
    <p class="mt-3 text-center mensagem">
      Você não possui tarefas pendentes.
    </p>
    </div>
    <ListaDeTarefas :tarefas="getTarefasFiltradas"/>
  </div>
</template>

<style>
.container {
  margin: 100px auto;
}

.container .mensagem{
  background-color: #d3f0d2;
  color: #5bad3b;
  font-size: 20px;
  border-radius: 3px
}

</style>

