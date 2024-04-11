<script setup>
    import { reactive } from 'vue';
    import Cabecalho from './components/Cabecalho.vue'
    import Formulario from './components/Formulario.vue'
    import ListaDeTarefas from './components/ListaDeTarefas.vue'

    const estado = reactive({
        filtro: 'todas',
        tarefaTemp: '',
        tarefas: [
        {
            titulo: 'Estudar ES6',
            finalizada: false,
        },
        {
            titulo: 'Estudas SASS',
            finalizada: false,
        },
        {
            titulo: 'Ir para academia',
            finalizada: true,
        }
        ]
    })

    const getTarefasFinalizadas = () => {
        return estado.tarefas.filter(tarefa => tarefa.finalizada)
    }

    const getTarefasPendentes = () => {
        return estado.tarefas.filter(tarefa => !tarefa.finalizada)
    }

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
    }

    const cadastraTarefa = () => {
        const tarefaNova = {
            titulo: estado.tarefaTemp,
            finalizada: false,
        }
        estado.tarefas.push(tarefaNova);
        estado.tarefaTemp = '';
    }

    const tarefasPendentes = getTarefasPendentes();

</script>

<template>
    <div class="container">
        <Cabecalho :tarefasPendentes="getTarefasPendentes().length"/>
        <Formulario :trocarFiltro="evento => estado.filtro = evento.target.value" :tarefaTemp="estado.tarefaTemp" :editaTarefaTempo="evento => estado.tarefaTemp = evento.target.value" :cadastraTarefa="cadastraTarefa"/>
        <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
    </div>
</template>