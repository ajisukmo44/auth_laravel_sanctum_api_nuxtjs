<template>
 <nav class="navbar navbar-expand-md navbar-success bg-success mb-4">
   <div class="container-fluid" v-if="!$auth.loggedIn">
     <NuxtLink to="/" class="navbar-brand  text-white">HOME</NuxtLink>
        </div>
        <div class="container-fluid" v-if="$auth.loggedIn">
          <NuxtLink to="/dashboard" class="navbar-brand text-white">DASHBOARD</NuxtLink>
          </div>
          <ul class="navbar-nav me-auto mb-2 mb-md-0" v-if="!$auth.loggedIn">
            <li class="nav-item">
            <NuxtLink to="/auth/login" class="nav-link mr-2  text-white">LOGIN</NuxtLink>
            </li>
            <li class="nav-item">
            <NuxtLink to="/auth/register" class="nav-link  text-white">REGISTER</NuxtLink>
            </li>
            </ul>
            <ul class="navbar-nav me-auto  mb-md-0" v-if="$auth.loggedIn">
            <li class="nav-item">
            <a href="#" @click.prevent="logout" class="text-white">LOGOUT</a>
            </li>
          </ul>
  </nav>
</template>

<script>
 export default {
        methods: {
            logout() {
              this.$auth.logout()
            }
        }
    }
</script>