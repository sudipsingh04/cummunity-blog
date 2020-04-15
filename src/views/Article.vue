<template>
    <h1 class="text-center">
        Single article
    </h1>
</template>

<script>
import Axios from 'axios'
import config from '@/config'

export default {
    mounted() {
        this.getArticle()
    },

    methods: {
        getArticle(){
            Axios.get(`${config.apiUrl}/article/${this.$route.params.id}`).then(response => {
                console.log(response);
                
            })
        }
    }
}
</script>