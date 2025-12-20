<template>
 <div class="container" style="max-width: 800px;">
    <h1 class="text-center my-4">TodoList</h1>
  
    <TaskStats :tasks="tasks"/>

    <TaskInput @add-task="addNewTask"></TaskInput>

    <FilterTask 
      v-if="tasks.length"
      @search="onSearch"
      @status="onStatus"
    />

    <TaskList :tasks="tasksFiltered" @delete="deleteTask"/>

    <TaskEmtyState v-if="!tasksFiltered.length" :message="empytState"></TaskEmtyState>
</div>

</template>
<script setup>
import { ref, computed } from 'vue'
import TaskStats from './components/TaskStats.vue'
import FilterTask from './components/FilterTask.vue'
import TaskInput from './components/TaskInput.vue'
import TaskList from './components/TaskList.vue'
import TaskEmtyState from './components/TaskEmtyState.vue'

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
