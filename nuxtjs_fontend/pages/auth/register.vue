<template>
    <div class="container">
        <div class="card col-sm-4 pl-5 pr-5 pb-4 pt-3">
         <Logo />
         <form @submit.prevent="register" class="">
            <div class="form-group row">
                <input type="text" v-model="form.name" class=" form-control" placeholder="Name" required>
                <span class="text-danger" v-if="errors.name">{{ errors.name[0] }}</span>
            </div>
            <div class="form-group row">
                <input type="text" v-model="form.username" class=" form-control" placeholder="Username" required>
                <span class="text-danger" v-if="errors.username">{{ errors.username[0] }}</span>
            </div>
            <div class="form-group row">
                <input type="email" v-model="form.email" class=" form-control"  placeholder="Email" required>
                <span class="text-danger" v-if="errors.email">{{ errors.email[0] }}</span>
            </div>
            <div class="form-group row">
                <input type="password" v-model="form.password" class=" form-control" minlength="8" placeholder="Password" required>
                <span class="text-danger" v-if="errors.password">{{ errors.password[0]}}</span>
            </div>
            <div class="form-group row">
                <input type="password" v-model="form.password_confirmation" minlength="8" class=" form-control" placeholder="Confirm Password" required>
            </div>
            <div class="text-center row">
                <button type="submit" class="btn btn-success btn-block">Register</button>
                <nuxt-link to="login" class="btn btn-outline-success btn-block">Login</nuxt-link>
            </div>
        </form>
        </div>
    </div>
</template>

<script>
import Swal from 'sweetalert2'
    export default {
        middleware: 'guest',
        head: {
            title: ' xsukmo | Register',
            meta: [
                { hid: 'description', name: 'description', content: 'Registration page' }
            ],
        },
        data: ()=> ({
            form: {
                name: '',
                username: '',
                email: '',
                password: '',
                password_confirmation: '',
            },
            errors: [],
        }),
        methods: {
            async register() {
                try {
                    let errors = []
                    await this.$axios.$get('sanctum/csrf-cookie')
                    await this.$axios.$post('/register', this.form)
                    .then((resp) => {
                    const Toast = Swal.mixin({
                    toast: true,
                    position: 'top-end',
                    showConfirmButton: false,
                    timer: 3000,
                    timerProgressBar: false,
                    didOpen: (toast) => {
                        toast.addEventListener('mouseenter', Swal.stopTimer)
                        toast.addEventListener('mouseleave', Swal.resumeTimer)
                    }
                    })
                    Toast.fire({
                    icon: 'success',
                    title: 'Register Berhasil!'
                    })
                        })
                        .catch((err) => {
                            if (err.response.status = 422) {
                                errors = err.response.data.errors
                            }
                        })
                        this.errors = errors
                    await this.$auth.loginWith('laravelSanctum', {data: this.form})
                } catch (error) {}
            }
        }
    }
</script>
<style scoped>
.card {
  margin: auto;
  margin-top:7%;
  width: 50%;
  padding: 10px;
}
</style>