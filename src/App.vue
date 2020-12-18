<template>
  <div id="app" class="bg-gray-500100 h-screen">
    <users></users>
    
  </div>
</template>

<script>
import DeleteModal from './components/DeleteModal.vue'
import Users from "./components/Users.vue"


export default {
  components: { Users, DeleteModal },
  name: 'App',
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100vh;
}
</style>
