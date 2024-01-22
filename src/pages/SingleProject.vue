<template>
    <div v-if="project" class="p-3">
        <h1>{{ project.title }}</h1>
        <img :src="`${store.imgPath}${project.image}`" :alt="project.title">
        <p>{{ project.description }}</p>
        <div v-if="project.category" class="mb-3">
            <h3 class="mb-2">Categoria</h3>
            <div>{{ project.category.name }}</div>
        </div>
        <div v-if="project.technologies.length !== 0">
            <h3 class="mb-2">Tecnologie</h3>
            <div class="d-flex align-items-center">
                <div v-for="technology in project.technologies" class="rounded-pill text-bg-success p-2 me-2">{{
                    technology.name
                }}</div>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
import { store } from '../assets/data/store'
export default {
    name: 'SingleProject',
    data() {
        return {
            store,
            project: null,
        }
    },
    methods: {
        getPostData() {
            console.log(this.$route);
            axios.get(`${this.store.apiUrl}/projects/${this.$route.params.slug}`).then((res) => {
                console.log(res.data)
                if (res.data.results) {
                    this.project = res.data.results
                } else {
                    this.$router.push({ name: "not-found" })
                }

            })
        }
    },
    created() {
        this.getPostData();
    }
}
</script>

<style lang="scss" scoped></style>