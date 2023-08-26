<template>
  <div class="home">
  <h1>Home</h1>
  <filter-nav @filterChange="current = $event" :current="current"></filter-nav>
    <div v-if="projects.length">
        <div v-for="project in filteredProject" :key="project.id">
          
           <single-project @complete="handleComplete"  @delete="handleDelete" :project="project"></single-project>
        </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '@/components/FilterNav.vue';
export default {
  name: 'Home',
  components: {
      SingleProject,
      FilterNav
  },
  data(){
    return {
      projects : [],
      current:'all',
    }
  },
  methods:{
    handleDelete(id){
      this.projects = this.projects.filter((project)=> project.id !== id)
    },
    handleComplete(id){
      let p = this.projects.find(project => project.id === id);
      p.complete = !p.complete
    }

  },
  computed:{
    filteredProject(){
      if(this.current === 'complated'){
        return this.projects.filter(project => project.complete)
      }else if (this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }

      return this.projects;
    }
  },

  mounted(){
    fetch("http://localhost:3000/project")
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message));
  },
}
</script>


