<template>
  <div class="project">
    <div class="actions" @click="isVisible.value = !isVisible.value">
      <h3>{{ project.title }}</h3>
      <div class="icons">
        <span class="material-icons">edit</span>
        <span @click="deleteProject" class="material-icons">delete</span>
        <span @click="toggleComplete" class="material-icons">done</span>
      </div>
    </div>
    <div class="details" v-if="isVisible.value">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
import { computed, reactive } from 'vue'

export default {
  props: {
    project: Object
  },
  setup (props, { emit }) {
    const isVisible = reactive({
      value: false
    })

    const uri = 'http://localhost:3000/projects/' + props.project.id

    const deleteProject = () => {
      fetch(uri, { method: 'DELETE'})
        .then(() => emit('delete', props.project.id))
        .catch(err => console.log(err))
    }

    const toggleComplete = () => {
      fetch(uri, { 
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ complete: !props.project.complete })
      })
    }

    return {
      isVisible,
      deleteProject,
      uri,
      toggleComplete
  
    }
  }

  
}
</script>

<style scoped>
  .project {
    margin: 20px auto;
    background: #fff;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid #e90074;
  }

  h3 {
    cursor: pointer;
    animation: 0.5s;
  }

  .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
  }

  .material-icons:hover {
    color: #777;
  }
</style>