<template>
  <div>
    <ul>
      <li v-for="project in projects" :key="project.it">
        <h2>{{ project.title }}</h2>
        <p>{{ project.description }}</p>
      </li>
    </ul>
    <button @click="nextPage()" :disabled="currentPage === 4 ? true : false">Avanti</button>
    <button @click="previousPage()" :disabled="currentPage === 1 ? true : false">Indietro</button>
  </div>
</template>

<script>
import axios from "axios";
import { store } from './assets/data/store';
export default {
  name: "App",
  data() {
    return {
      store,
      projects: [],
      currentPage: 1,
      lastPage: 0
    }
  },
  methods: {
    getAllProjects() {
      axios.get(store.apiUrl + "/projects", { params: { page: this.currentPage } }).then((res) => {
        console.log(res.data);
        this.projects = res.data.results.data;
        console.log(this.projects);
        this.currentPage = res.data.results.current_page;
        this.lastPage = res.data.results.last_page
      })
    },
    nextPage() {
      this.currentPage += 1;
      this.getAllProjects();
    },
    previousPage() {
      this.currentPage -= 1;
      this.getAllProjects();
    }
  },
  mounted() {
    this.getAllProjects()
  }
}
</script>

<style lang="scss" scoped></style>