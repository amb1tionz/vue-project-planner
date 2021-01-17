<template>
  <div class="home" :state="state">
    <FilterNav @filterChange="state.current = $event" :current="state.current"/>
    <div v-if="state.projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>

import { reactive, onMounted, computed } from 'vue'
import SingleProject from '../components/SingleProject'
import FilterNav from '../components/FilterNav'

export default {
  setup () {
    const state = reactive({
      projects: [],
      current: 'all'
    })

    console.log(state.current)
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

    const filteredProjects = computed(() => {
      if (state.current === 'completed') {
        return state.projects.filter(project => project.complete)
      }
      if (state.current === 'ongoing') {
        return state.projects.filter(project => !project.complete)
      }
      return state.projects
    })

    return {
      state,
      onMounted,
      SingleProject,
      handleDelete,
      handleComplete,
      FilterNav,
      filteredProjects
    }
  }
}
</script>
