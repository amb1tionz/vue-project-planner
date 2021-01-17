<template>
  <div class="home" :state="state">
    <div v-if="state.projects.length">
      <div v-for="project in state.projects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>

import { reactive, onMounted } from 'vue'
import SingleProject from '../components/SingleProject'

export default {
  setup () {
    const state = reactive({
      projects: []
    })

    onMounted(() => {
      fetch('http://localhost:3000/projects')
        .then(res => res.json())
        .then(data => {
          state.projects = data
          
        })
        .catch(err => console.log(err.message))
    })

    const handleDelete = (id) => {
      state.projects = state.projects.filter((project) => {
        return project.id !== id
      })
    }

    const handleComplete = (id) => {
      let p = state.projects.find(project => {
        return project.id === id
      })
      p.complete = !p.complete
    }

    return {
      state,
      onMounted,
      SingleProject,
      handleDelete,
      handleComplete
    }
  }
}
</script>
