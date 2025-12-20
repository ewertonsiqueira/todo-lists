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
		Adicionar
	</button>
</div>
</template>
<script setup>
import { ref, defineEmits } from 'vue';

const emits = defineEmits(['add-task'])
const task = ref('')

const addNewTask = () => {
	if(!task.value) return
	emits('add-task', task.value) 
}
</script>