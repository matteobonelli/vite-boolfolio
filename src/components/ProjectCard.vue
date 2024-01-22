<template>
    <div class="card">
        <img v-if="project" :src="`${store.imgPath}${project.image}`" :alt="project.title">

        <router-link :to="{ name: 'single-project', params: { slug: project.slug } }" class="btn btn-primary">
            <h2>{{ project.title }}</h2>
        </router-link>
    </div>
</template>

<script>
import { store } from '../assets/data/store'
export default {
    name: "ProjectCard",
    props: ['project'],
    data() {
        return {
            store
        }
    }
}
</script>

<style lang="scss" scoped></style>