<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions" >
      <h3 @click="isVisible.value = !isVisible.value">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id }}">
          <span class="material-icons">edit</span>
        </router-link>
        <span @click="deleteProject" class="material-icons">delete</span>
        <span @click="toggleComplete" class="material-icons tick">done</span>
      </div>
    </div>
    <div class="details" v-if="isVisible.value">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue'

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
      }).then(() => {
        emit('complete', props.project.id)
      }).catch(err => console.log(err))
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
    transition: 0.5s;
  }

  .project.complete {
    border-left: 4px solid #00ce89;
    transition: 0.5s
  }

  .project.complete .tick {
    color: #00ce89;
    transition: 0.5s;
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