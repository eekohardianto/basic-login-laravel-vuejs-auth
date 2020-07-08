<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                Register
            </div>
            <div class="card-body">
                <div class="col-md-6 offset-md-3">
                    <form v-on:submit.prevent="onSubmit">
                        <div class="alert alert-danger" v-if="errors.length">
                            <ul class="mb-0">
                                <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
                            </ul>
                        </div>
                        <div class="form-group">
                            <label>Name</label>
                            <input type="text" class="form-control" placeholder="Name..." v-model="name">
                        </div>
                        <div class="form-group">
                            <label>Email</label>
                            <input type="email" class="form-control" placeholder="Email..." v-model="email">
                        </div>
                        <div class="form-group">
                            <label>Password</label>
                            <input type="password" class="form-control" placeholder="Password..." v-model="password">
                        </div>
                        <div class="form-group">
                            <label>Confirm Password</label>
                            <input type="password" class="form-control" placeholder="Confirm Password..."
                                v-model="confirmPassword">
                        </div>
                        <button class="btn btn-success">Register</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        name: 'register',
        props: ['app'],
        data() {
            return {
                name: '',
                email: '',
                password: '',
                confirmPassword: '',
                errors: []
            }
        },
        methods: {
            onSubmit() {
                this.error = [];
                if (!this.name) {
                    this.errors.push('Name is required.');
                }
                else if (!this.email) {
                    this.errors.push('Email is required.');
                }
                else if (!this.password) {
                    this.errors.push('Password is required.');
                }
                else if (!this.confirmPassword) {
                    this.errors.push('Password confirmation is required.');
                }
                else if (this.password !== this.confirmPassword) {
                    this.errors.push('Passwords do not match.');
                }
                else {
                    const data = {
                        name: this.name,
                        email: this.email,
                        password: this.password
                    }

                    this.app.req
                        .post("auth/register", data)
                        .then(response => {
                            this.app.user = response.data;
                            this.$router.push("/");
                        })
                        .catch(error => {
                            this.errors.push(error.response.data.error);                            
                        });
                }
            }
        }
    }

</script>
<style>

</style>
