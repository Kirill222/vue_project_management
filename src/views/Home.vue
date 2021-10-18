<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete(project.id)" @complete="handleComplete(project.id)"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'

export default {
  name: 'Home',
  data() {
    return {
      projects: []
    }
  },  
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter(p => {
        return p.id != id
      })
    },
    handleComplete(id) {
      let p = this.projects.find(p => {
        return p.id === id
      })
      p.complete = !p.complete

      //second method to implement the same logic
      // this.projects = this.projects.filter(p => {
      //   if(p.id == id) {
      //     p.complete = !p.complete
      //   }
      //   return this.projects
      // })
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  },
  components: {
    SingleProject,
  },
}
</script>
