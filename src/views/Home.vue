<template>
  <div class="home">
  <h1>Home</h1>
    <div v-if="projects.length">
        <div v-for="project in projects" :key="project.id">
          
           <single-project @complete="handleComplete"  @delete="handleDelete" :project="project"></single-project>
        </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';
export default {
  name: 'Home',
  components: {
      SingleProject
  },
  data(){
    return {
      projects : []
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

  mounted(){
    fetch("http://localhost:3000/project")
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.log(err.message));
  },
}
</script>


