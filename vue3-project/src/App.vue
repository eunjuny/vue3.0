<template>
  <div class="name">{{ name }}</div>
  <button 
  class="btn btn-primary"
  v-on:click="updateName">
  Click
  </button>
</template>

<script>
import { ref } from 'vue';

export default {
  setup(){
    let name = ref("Kossie");
    
    // const greeting = () => {
    //   return 'Hello, ' + name;
      
    // };
    const updateName = () => {
      name.value = 'Coder';
    }

    return {
      name,
      updateName
    };
  }
}
</script>

<style>
</style>
