<template>
    <main class="container">
        <h1>Projects List</h1>
        <div class="row">
            <div class="col-12 col-md-4 col-lg-3" v-for="project in store.projects" :key="project.id">
                <ProjectCard :project="project" />
            </div>
            <select name="category" id="category" v-model="selectedCategory" @change="getAllProjects(currentPage)">
                <option value="">Tutte</option>
                <option v-for="category in store.categories" :value="category.id">{{ category.name }}</option>
            </select>
            <button @click="getAllProjects(currentPage + 1)"
                :disabled="currentPage === lastPage ? true : false">Avanti</button>
            <ul class="d-flex">
                <li v-for="n in lastPage">
                    <button @click="getAllProjects(n)">{{ n }}</button>
                </li>
            </ul>
            <button @click="getAllProjects(currentPage - 1)" :disabled="currentPage === 1 ? true : false">Indietro</button>
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
            lastPage: 0,
            selectedCategory: ''
        }
    },
    components: {
        ProjectCard
    },
    methods: {
        getAllProjects(n) {
            if (this.selectedCategory === '') {
                axios.get(this.store.apiUrl + "/projects", { params: { page: n } }).then((res) => {
                    console.log(res.data);
                    this.store.projects = res.data.results.data;
                    this.currentPage = res.data.results.current_page;
                    this.lastPage = res.data.results.last_page
                })
            } else {
                this.currentPage = 1;
                axios.get(this.store.apiUrl + "/projects", { params: { page: n, category_id: this.selectedCategory } }).then((res) => {
                    console.log(res.data);
                    this.store.projects = res.data.results.data
                    this.currentPage = res.data.results.current_page
                    this.lastPage = res.data.results.last_page
                })
            }

        },
        getAllCategories() {
            if (this.store.categories.length < 1) {
                axios.get(this.store.apiUrl + "/categories").then((res => {
                    //console.log(res.data);
                    this.store.categories = res.data.results;
                    console.log(this.store.categories);
                }))
            } else {
                return
            }

        }
    },
    mounted() {
        this.getAllProjects(this.currentPage)
        this.getAllCategories();
    }
}
</script>

<style lang="scss" scoped>
ul {
    list-style: none;
}
</style>