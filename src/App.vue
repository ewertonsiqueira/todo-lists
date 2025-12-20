<template>
 <div class="container" style="max-width: 800px;">
    <h1 class="text-center my-4">TodoList</h1>
  
    <TaskStats :tasks="tasks"/>
    
    <!-- Add new task -->
    <TaskInput @add-task="addNewTask"></TaskInput>

    <!-- Filters -->
    <FilterTask 
      v-if="tasks.length"
      @search="onSearch"
      @status="onStatus"
    />

    <!-- Tasks -->
    <ul class="list-group">
      <li
        v-for="task in tasksFiltered"
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
     <div v-if="!tasksFiltered.length" class="card bg-light">
      <div class="card-body text-center py-5">
        <p class="text-muted mb-0">{{ empytState }}</p>
      </div>
    </div> 
</div>

</template>
<script setup>
import { ref, computed } from 'vue'
import TaskStats from './components/TaskStats.vue'
import FilterTask from './components/FilterTask.vue'
import TaskInput from './components/TaskInput.vue'

  const tasks = ref([])
  const filterSearchs = ref('')
  const filterStatus = ref('')

  const tasksFiltered = computed(() => {
    let result = tasks.value
    
    if (filterSearchs.value) {
      const search = filterSearchs.value.toLowerCase()
      result = result.filter(task => task.name.toLowerCase().includes(search))
    }
   
    if (filterStatus.value === 'pending') {
      return result.filter(task => task.completed === false)
    } else if (filterStatus.value === 'completed') {
      result = result.filter(task => task.completed === true)
    }
  
    return result
  })

  const onSearch = (search) => {
    filterSearchs.value = search
  }

  const onStatus = (status) => {
    filterStatus.value = status
  }


  const addNewTask = (task) => {
   if (!task) return

     tasks.value.push({
      id: Date.now(),
      name: task,
      completed: false,
      state: 'show'
    })  
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

  const empytState = computed(() => {
    const output = 'Nenhuma tarefa no momento'

    if (filterStatus.value ||  filterSearchs.value) {
      return 'Nenhuma tafera com esse filtro.'
    }
    return output
  })

</script>
<style scoped>

</style>
