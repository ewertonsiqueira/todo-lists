<template>
<div class="card mb-3">
	<div class="card-body">
		<div class="row text-center">
			<div class="col-4">
				<div class="fw-bold fs-4">{{ totalTask }}</div>
				<div class="text-muted small">Total</div>
			</div>
		<div class="col-4">
			<div class="fw-bold fs-4 text-success">{{ totalCompleted }}</div>
			<div class="text-muted small">Concluídas</div>
		</div>
		<div class="col-4">
			<div class="fw-bold fs-4 text-warning"> {{ totalPending }}</div>
			<div class="text-muted small">Pendentes</div>
		</div>
	</div>
</div>
</div>
</template>

<script setup>
	import { defineProps, computed } from 'vue';

	const props = defineProps(['tasks']) 

	const totalTask = computed(() => props.tasks.length || 0)
	const totalCompleted = computed(() => props.tasks.filter(task => task.completed).length || 0)
	const totalPending = computed(() => props.tasks.filter(task => !task.completed).length || 0)

</script>