<template>
    <div class="container">
        <div class="card col-sm-4 pl-5 pr-5 pb-4 pt-3">
            <Logo />
            <form @submit.prevent="login" class="">
            <div class="form-group row">
            <input type="email" v-model="form.email" class=" form-control" placeholder="Email" required>
            </div>
            <div class="form-group row">
            <input :type="passwordFieldType" v-model="form.password" class=" form-control" minlength="8" placeholder="Password" required>
            </div>   
            <div class="form-group row">
            <input type="checkbox" class="mt-1"  @click="switchVisibility">&nbsp;<span class="text-secondary">Show Password</span>
            </div>
            <div class="text-center row">
            <button type="submit" class="btn btn-success btn-block">Login</button>
            <nuxt-link to="register" class="btn btn-outline-success btn-block">Register</nuxt-link>
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
            title: ' xsukmo | Login',
            meta: [
                { hid: 'description', name: 'description', content: 'Login page' }
            ],
        },
        data:() => ({
      password: '',
      passwordFieldType: 'password',
            form: {
                email: '',
                password: ''
            },
            errors: ''
        }),
        methods: {
            async login() {
                try {
                    await this.$auth.loginWith('laravelSanctum', {data:this.form})
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
                    title: 'Login Berhasil!'
                    })
                } catch (err) {
                    if (err.response.status = 422) {
                    Swal.fire({
                    icon: 'error',
                    title: 'Login Gagal!',
                    text: 'Email & Password Tidak Sesuai.',
                    confirmButtonColor: '#F27474',
                    })
                    }
                }
            },
            switchVisibility() {
                this.passwordFieldType = this.passwordFieldType === "password" ? "text" : "password";
                }
        }
    }
</script>

<style scoped>
.card {
  margin: auto;
  margin-top:10%;
  width: 50%;
  padding: 10px;
}
</style>