<template>
    <div class="row my-5">
        <div class="col-md-6 offset-md-3">
            <div class="card">
                <div class="card-body">
                    <h3 class="text-center my-4">Login</h3>

                    <div class="form-group">
                        <input v-bind:class="{ 'is_invalid': errors.email }" v-model="email" type="text" class="form-control" placeholder="Email">
                        <div class="errors" v-if="errors.email">
                            <small class="text-danger" v-for="error in errors.email" :key="error">{{ error }}</small>
                        </div>
                    </div>

                    <div class="form-group">
                        <input v-bind:class="{ 'is_invalid': errors.password }" v-model="password" type="password" class="form-control" placeholder="Password">
                        <div class="errors" v-if="errors.password">
                            <small class="text-danger" v-for="error in errors.password" :key="error">{{ error }}</small>
                        </div>
                    </div>

                    <div class="form-group text-center">
                        <button @click="loginUser()" :disabled="loading" class="form-control btn btn-success">
                            <i class="fa fa-spin fa-spinner" v-if="loading"></i>
                            {{ loading ? '' : 'Login' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.is_invalid{
    border-color: #a94442;
}
.is_valid{
    border-color: #3c763d;
}
</style>

<script>
import Axios from 'axios'

export default {
    data() {
        return {
            email: '',
            password: '',
            errors: {},
            loading: false,
        }
    },

    methods: {
        loginUser(){
            this.loading = true
            Axios.post('https://react-blog-api.bahdcasts.com/api/auth/login', {
                email: this.email,
                password: this.password

            }).then((response) => {
                this.loading = false
                this.$root.auth = response.data.data
                localStorage.setItem('auth', JSON.stringify(response.data.data))
                this.$router.push('home')

            }).catch(({ response }) => {
                this.loading = false
                if(response.status == 401){
                    this.errors = {
                        email: ["These credentials do not match our records"]
                    }
                }else{
                this.errors = response.data
                }
            })
        }
    }
}
</script>