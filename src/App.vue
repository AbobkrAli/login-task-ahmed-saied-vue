<!-- src/App.vue -->
<template>
  <div id="app" class="bg-gray-200 min-h-[100vh] min-w-[100vw] flex justify-center items-center">
    <RegisterForm />
  </div>
</template>

<script>
import RegisterForm from './views/RegisterForm.vue';

export default {
  name: 'App',
  components: {
    RegisterForm
  }
};
</script>

<style>
</style>
