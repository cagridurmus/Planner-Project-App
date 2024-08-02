<template>
  <div class="home">
    <FilterNav @filterChanged="currentFilter = $event"  :currentFilter="currentFilter"/>
    <div v-if="this.projects.length">
        <div v-for="project in filteredProjects" :key="project.id">
            <Project :project="project" @delete="handleDelete" @complete="handleComplete"></Project>
        </div>
    </div>
  </div>
</template>

<script>
  import Project from '../components/Project.vue'
  import FilterNav from '../components/FilterNav.vue'
  export default {
    components: {
      Project,
      FilterNav
    },
    data(){
      return {
        projects: [],
        currentFilter: 'all'
      }
    },
    mounted() {
      fetch('http://localhost:3000/projects')
        .then(res => res.json())
        .then(data => this.projects = data)
        .catch(err => console.log(err.message))
    },
    methods: {
      handleDelete(id){
        this.projects = this.projects.filter((project) => project.id !== id)
      },
      handleComplete(id){
        let project = this.projects.find(project => project.id === id)
        project.completed = !project.completed;
      }
    },
    computed:{
      filteredProjects(){
        if(this.currentFilter == 'completed')
          return this.projects.filter((project) => project.completed)
        else if(this.currentFilter == 'ongoing')
          return this.projects.filter((project) => !project.completed)
        else
          return this.projects
      }
    }
  }
</script>


