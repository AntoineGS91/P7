<template>
    <div>
        <NavCo />
        <h1 class="mt-15 mb-15 text-center">Bonjour à vous, {{ username }}</h1>
        <div class="mt-15">
            <v-form method="PUT" @submit.prevent ="publishComment" id="container" class="mx-auto pl-3 pr-3 rounded-lg">
                <v-text-field
                    class="mb-10 mt-5"
                    label="Titre"
                    id="post_title"
                    placeholder="Le titre de mon article"
                    hide-details="auto"
                    required>
                </v-text-field>
                <v-textarea
                    class="mb-10 mt-5"
                    label="Contenu"
                    id="post_content"
                    placeholder="Le contenu de mon article"
                    hide-details="auto"
                    required>
                </v-textarea>
                <v-btn
                    type="submit"
                    class="text-center mb-3"
                    depressed
                    color="lime">
                    Modifier le post</v-btn>
            </v-form>
        </div>
    </div>
</template>

<script>
import axios from "axios"
import NavCo from "../components/NavCo.vue"

export default {
    name : "NewPost",
    components: { 
        NavCo,
    },
    data() {
        return {
            username: '',
            token: ''
        }
    },
    mounted() {
    const user = JSON.parse(localStorage.getItem('user'))
        if (user) {
            this.id = user.id
            this.username = user.username
            this.token = user.token
        } else this.$router.push({ name: 'login' })
    },
    methods: {
        publishComment() {
            const user = JSON.parse(localStorage.getItem('user')) 
            const postTitle = document.querySelector("#post_title").value
            const postContent = document.querySelector("#post_content").value
            const post = JSON.stringify({'content': postContent, 'title' : postTitle, 'userId': user.id})
        axios
            .put("http://localhost:3000/api/post/" + post.id, post,
                {headers: {
                "Content-Type": "application/json",
                Authorization: "Bearer " + user.token}})
            .then(() => {
                this.$router.push( '/feed' )})
            .catch((error) => {
                console.log(error)
            })
        },
    }
}       
</script>

<style>

</style>