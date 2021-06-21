<template>
  <div>
    <Nav />
    <div class=" justify-content-center">
    <H1>HELLO, {{ this.$auth.user.username }}</H1>
    </div>
  </div>
</template>

<script>
 export default {
     middleware: 'auth',
     }
</script>