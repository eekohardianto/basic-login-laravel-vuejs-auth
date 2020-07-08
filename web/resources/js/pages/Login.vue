<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Login
            </div>
            <div class="card-body">
                <div class="col-md-6 offset-md-3">
                    <spinner v-if="loading"></spinner>
                    <form v-on:submit.prevent="onSubmit" v-if="loginform">
                        <div class="alert alert-danger" v-if="errors.length">
                            <ul class="mb-0">
                                <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
                            </ul>
                        </div>
                        <div class="form-group">
                            <label>Email</label>
                            <input type="email" class="form-control" placeholder="Email..." v-model="email">
                        </div>
                        <div class="form-group">
                            <label>Password</label>
                            <input type="password" class="form-control" placeholder="Password..." v-model="password">
                        </div>                        
                        <button type="submit" class="btn btn-success">Login</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'login',
        props: ['app'],
        data() {
            return {
                email: '',
                password: '',
                loading: false,
                loginform: true, 
                errors: []
            }
        },
        methods: {
            onSubmit() {           
                this.error = [];
                if (!this.email) {
                    this.errors.push('Email is required.');
                }
                else if (!this.password) {
                    this.errors.push('Password is required.');
                }
                else {
                    const data = {
                        email: this.email,
                        password: this.password
                    }

                    this.app.req
                        .post("auth/login", data)
                        .then(response => {
                            this.app.user = response.data;
                            //$('.login-form').remove();
                            this.loginform = false;                           
                            this.loading = true;
                            setTimeout(() => {
                                this.$router.push("/");
                            }, 3000)                                                   
                        })
                        .catch(error => {
                            this.errors.push(error.response.data.error);                           
                        });
                }
                
            }
        }
}
</script>