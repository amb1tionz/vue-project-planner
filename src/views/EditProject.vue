<template>
    <form>
    <label>Title:</label>
    <input type="text" required v-model="state.title">
    <label>Details:</label>
    <textarea required v-model="state.details"></textarea>
    <button @click.prevent="updateProject">Update Project</button>
  </form>
</template>

<script>
import { reactive, onMounted } from 'vue'
import { useRouter } from 'vue-router'

export default {
  props: {
    id: String
  },
  setup (props) {
    const state = reactive({
      title: '',
      details: '',
      uri: 'http://localhost:3000/projects/' + props.id
    })

    const router = useRouter()

    onMounted(() => {
      fetch(state.uri)
        .then(res => res.json())
        .then(data => {
          state.title = data.title
          state.details = data.details
        })
    })

    const updateProject = () => {
      console.log('Fetching...')
      fetch(state.uri, { 
        headers: { 'Content-Type': 'application/json' },
        method: 'PATCH',
        body: JSON.stringify({
          title: state.title,
          details: state.details
        })
      }).then(() => {
          router.push('/')
      }).catch((err) => {
        console.log(err)
      })
    }

    return {
      state,
      onMounted,
      updateProject,
      router
    }
  }
}
</script>

<style>

</style>