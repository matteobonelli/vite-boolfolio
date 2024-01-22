<template>
    <main class="container">
        <h1>Projects List</h1>
        <div class="row">
            <div class="col-12 col-md-4 col-lg-3" v-for="project in store.projects" :key="project.id">
                <ProjectCard :project="project" />
            </div>
            <button @click="nextPage()" :disabled="currentPage === 4 ? true : false">Avanti</button>
            <button @click="previousPage()" :disabled="currentPage === 1 ? true : false">Indietro</button>
        </div>
    </main>
</template>

<script>
import axios from "axios";
import { store } from '../assets/data/store';
import ProjectCard from '../components/ProjectCard.vue';
export default {
    name: "AppProjects",
    data() {
        return {
            store,
            currentPage: 1,
            lastPage: 0
        }
    },
    components: {
        ProjectCard
    },
    methods: {
        getAllProjects() {
            axios.get(this.store.apiUrl + "/projects", { params: { page: this.currentPage } }).then((res) => {
                console.log(res.data);
                this.store.projects = res.data.results.data;
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