<template>
<ul class="list-group">
	<li
		v-for="task in tasks"
		:key="task.id"
		class="list-group-item d-flex align-items-center gap-2"
	>
		<template v-if="task.state === 'show'">
			<input
				v-model="task.completed"
				type="checkbox"
				class="form-check-input">
			<span
				:class="{
					'flex-grow-1': true,
					'text-decoration-line-through text-muted': task.completed}">
				{{ task.name }}</span>
			<button
				class="btn btn-primary btn-sm" @click="editTask(task)">Editar</button>
			<button class="btn btn-danger btn-sm" @click="task.state = 'delete'">Excluir</button>
		</template>

		<template v-else-if="task.state === 'edit'">
			<input v-model="task._completed" type="checkbox" class="form-check-input">
			<input  v-model="task._name" type="text" value="Corrigir bug no modal" class="form-control form-control-sm">
			<button class="btn btn-success btn-sm" @click="saveTask(task)">Salvar</button>
			<button class="btn btn-secondary btn-sm" @click="task.state = 'show'">Cancelar</button>
		</template>

		<template v-else-if="task.state === 'delete'">
			<span class="flex-grow-1">
				<div class="fw-semibold">{{ task.name }}</div>
				<div class="text-muted small">Tem certeza que deseja remover?</div>
			</span>
			<button class="btn btn-danger btn-sm" @click="deleteTask(task)">Sim, excluir</button>
			<button class="btn btn-outline-secondary btn-sm" @click="task.state = 'show'">Cancelar</button>
		</template>
		</li>
	</ul>
</template>
<script setup>
const props = defineProps(['tasks'])
const emit = defineEmits(['delete'])

const editTask = (task) => {
	if (!Object.hasOwn(task, '_name')) {
	task._name = task.name
	}
	if (!Object.hasOwn(task, '_completed')) {
	task._completed = task.completed
	}

	task.state = 'edit'
}


const saveTask = (task) => {
	task.name = task._name
	task.completed = task._completed
	task.state = 'show'

	delete task._name
	delete task._completed
}

const deleteTask = (task) => {
	emit('delete', task)
}
</script>