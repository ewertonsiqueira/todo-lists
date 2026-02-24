<template>
 <div class="input-group mb-3">
	<input
		v-model="task" 
		@keyup.enter="addNewTask"
		placeholder="Adicionar uma nova tarefa..."
		class="form-control"
	>
	<button 
		class="btn btn-success"
		@click="addNewTask"
	>
		<i v-if="loading" class="fa fa-spinner fa-spin"></i>
		Adicionar
	</button>
</div>
</template>
<script setup>
import { ref, defineEmits } from 'vue';

const emits = defineEmits(['add-task'])
const task = ref('')
const loading = ref(false)

const addNewTask = () => {
	if(!task.value) return
	loading.value = true
	console.log('Adicionando tarefa:', loading.value)
	setTimeout(() => {
		emits('add-task', task.value) 
		task.value = ''
	}, 500)
	loading.value = false
}
</script>