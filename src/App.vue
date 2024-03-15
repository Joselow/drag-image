<script setup lang="ts">
import DDImage from './components/DDImage.vue';
interface FileI {
    file: File,
    name: string,
    url: string,
}

const getImage = (data: FileI) => {
  console.log('Lo tengo owo');
  
  console.log(data);  
}
</script>

<template>
  <div>
    <DDImage 
      @return="getImage"
    />
  </div>
</template>

<style scoped>
</style>
