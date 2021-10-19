<template>
  <div class="home">    
  <FilterNav @filterChange="current = $event" :current="current"/>  <!-- $event - the data that were sent by custom event 'filterChange' -->
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete(project.id)" @complete="handleComplete(project.id)"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'


export default {
  name: 'Home',
  data() {
    return {
      projects: [],
      current: 'all',
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
    },    
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  },
  computed: {
      filteredProjects() {

        if(this.current === 'completed') {
          return this.projects.filter(p => p.complete)
        }
        if(this.current === 'ongoing') {
          return this.projects.filter(p => !p.complete)
        }
        return this.projects    
        }
      },
  components: {
    SingleProject,
    FilterNav,
  },
}
</script>

<style>
  
</style>