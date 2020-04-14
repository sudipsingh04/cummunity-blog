<template>
    <div class="row my-5">
        <div class="col-md-6 offset-md-3">
            <div class="card">
                <div class="card-body">
                    <h3 class="text-center my-4">Signup</h3>

                    <div class="form-group">
                        <input v-bind:class="{ 'is_invalid': errors.name, 'is_valid': !errors.name && this.submitted }" v-model="name" type="text" class="form-control" placeholder="Name">
                        <div class="errors" v-if="errors.name">
                            <small class="text-danger" v-for="error in errors.name" :key="error">{{ error }}</small>
                        </div>
                    </div>

                    <div class="form-group">
                        <input v-bind:class="{ 'is_invalid': errors.email, 'is_valid': !errors.email && this.submitted }" v-model="email" type="text" class="form-control" placeholder="Email">
                        <div class="errors" v-if="errors.email">
                            <small class="text-danger" v-for="error in errors.email" :key="error">{{ error }}</small>
                        </div>
                    </div>

                    <div class="form-group">
                        <input v-bind:class="{ 'is_invalid': errors.password, 'is_valid': !errors.password && this.submitted }" v-model="password" type="password" class="form-control" placeholder="Password">
                        <div class="errors" v-if="errors.password">
                            <small class="text-danger" v-for="error in errors.password" :key="error">{{ error }}</small>
                        </div>
                    </div>

                    <div class="form-group text-center">
                        <button @click="registerUser" :disabled="loading" class="form-control btn btn-success">
                            <i class="fa fa-spin fa-spinner" v-if="loading"></i>
                            {{ loading ? '' : 'Signup' }}
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
            name: '',
            email: '',
            password: '',
            errors: {},
            submitted: false,
            loading: false,
        }
    },

    methods: {
        registerUser() {
            this.loading = true
            Axios.post('https://react-blog-api.bahdcasts.com/api/auth/register', {
                name: this.name,
                email: this.email,
                password: this.password

            }).then((response) => {
                this.loading = false
                this.submitted = true
                const { data } = response.data;
                localStorage.setItem('auth', JSON.stringify(data))
                this.$root.auth = data

                this.$router.push('home')

            }).catch(({ response }) => {
                this.loading = false
                this.submitted = true
                this.errors = response.data                
            })
            
        }
    }
}
</script>