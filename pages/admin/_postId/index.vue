<template>
    <div class="admin-post-page">
        <section class="update-form">
            <AdminPostForm :post="loadedPost" @submit="onSubmitted"/>
        </section>
    </div>
</template>

<script>
    import axios from 'axios'
    import AdminPostForm from "@/components/Admin/AdminPostForm";

    export default {
        layout: 'admin',
        components: {
            AdminPostForm
        },
        asyncData(context) {
            return axios
                .get('https://nuxt-blog-67fb3.firebaseio.com/posts/' + context.params.postId + '.json')
                .then(res => {
                    return {
                        loadedPost: {...res.data, id: context.params.id}
                    }
                })
                .catch(e => context.error(e))

        },
        methods: {
            onSubmitted(editedPost) {
                this.$store.dispatch('editPost', editedPost)
                    .then(() => {
                        this.$router.push("/admin")
                    })
            }
        }
    }
</script>
