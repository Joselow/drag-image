<script setup lang="ts">
import DDImage from './components/DDImage.vue';
import DDSImage from './components/DDSImage.vue';

interface FileI {
    file?: File,
    name: string,
    url: string,
}

const getImage = (data: FileI) => {
  console.log('Lo tengo owo');
  
  console.log(data);  
}
const owo = (data: FileI) => {
  console.log(data);
  
}
</script>

<template>
  <div>
    <DDSImage
      @return="owo"

    />
    <DDImage 
      @return="getImage"
    />
  </div>
</template>

<style scoped>
</style>
