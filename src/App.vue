<template>
 <div class="container" style="max-width: 800px;">
    <h1 class="text-center my-4">Tarefinha</h1>
    
    <!-- Stats -->
    <!-- <div class="card mb-3">
      <div class="card-body">
        <div class="row text-center">
          <div class="col-4">
            <div class="fw-bold fs-4">3</div>
            <div class="text-muted small">Total</div>
          </div>
          <div class="col-4">
            <div class="fw-bold fs-4 text-success">1</div>
            <div class="text-muted small">Concluídas</div>
          </div>
          <div class="col-4">
            <div class="fw-bold fs-4 text-warning">2</div>
            <div class="text-muted small">Pendentes</div>
          </div>
        </div>
      </div>
    </div> -->
    
    <!-- Add new task -->
    <div class="input-group mb-3">
      <input
        v-model="newTask" 
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
      <pre>{{ tasks }}</pre>


    <!-- Filters -->
    <!-- <div class="d-flex gap-2 mb-3">
      <input type="text" placeholder="Buscar tarefa..." class="form-control" style="flex: 1;">
      <select class="form-select" style="flex: 1;">
        <option value="">Todas</option>
        <option value="pending">Pendentes</option>
        <option value="completed">Concluídas</option>
      </select>
      <button class="btn btn-outline-secondary btn-sm" style="flex-shrink: 0;">Limpar filtros</button>
    </div> -->

    <!-- Tasks -->
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

      <template v-else>
        <div class="card bg-light">
          <div class="card-body text-center py-5">
            <p class="text-muted mb-0">Nenhuma tarefa cadastrada</p>
          </div>
        </div> 
      </template>
      </li>
    </ul>
</div>

</template>
<script setup>
  import { ref } from 'vue'

  const tasks = ref([])
  const newTask = ref('')

  function addNewTask() {
   if (!newTask.value.trim()) return
   tasks.value.push({
    id: Date.now(),
    name: newTask.value,
    completed: false,
    state: 'show'
  })  
   newTask.value = ''
  }

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
    const index = tasks.value.findIndex(t => t.id === task.id)
    if (index !== -1) {
      tasks.value.splice(index, 1)
    }
  }

</script>
<style scoped>

</style>
