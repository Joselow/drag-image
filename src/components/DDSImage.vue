<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue';

interface ImageI {
  file?: File,
  name: string,
  url: string,
}

const emits = defineEmits<{
  return: [ImageI]
}>()

const props = defineProps<{
  url?: string
}>()

const container = ref<HTMLDivElement | null>(null)
const image = ref<ImageI>({
  url: '',
  name: '',
})

const dropFunction = (e: DragEvent) => {
  e.preventDefault();
  const file = e.dataTransfer?.files[0]

  if (!file) {
    showMsgError()
    return
  }

  if (!isvalidFile(file)) {
    showMsgError()
    return
  }

  const imgData = createInfo(file)
  image.value.url = imgData.url
  image.value.name = imgData.name

  emits('return', imgData)
}

const isvalidFile = (file: File | undefined) => {
  if (!file) return false

  const isImage = file.type.startsWith('image/')
  if (isImage) {
    return true
  }
  return false
}

const showMsgError = () => {
  console.log('solo se aceptan imgs');
  return
}

const createInfo = (file: File): ImageI => {
  const url = URL.createObjectURL(file)
  return {
    url,
    name: file.name,
    file
  }
}

const dragoverFunction = (e: DragEvent) => {
  e.preventDefault();
}

onMounted(() => {
  container.value?.addEventListener("dragover", dragoverFunction);
  container.value?.addEventListener("drop", dropFunction)
})

onUnmounted(() => {
  window.removeEventListener('dragover', dragoverFunction)
  window.removeEventListener('drop', dropFunction)
})

const urlIMG = computed(() => {
  return image.value.url ?? props.url
})
</script>

<template>
  <!-- <img src="/assets/a.png" />  -->
  <div class="image-container"
    ref="container"
  >
    <template v-if="image.url">
      <div class="image-wrapper">
        <img :src="urlIMG" :alt="image?.name" draggable="false">
      </div>
    </template>
    <template v-else>
      <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" fill="#000000">
        <g id="SVGRepo_bgCarrier" stroke-width="0"></g>
        <g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g>
        <g id="SVGRepo_iconCarrier">
          <g>
            <path fill="none" d="M0 0h24v24H0z"></path>
            <path fill-rule="nonzero"
              d="M16 13l6.964 4.062-2.973.85 2.125 3.681-1.732 1-2.125-3.68-2.223 2.15L16 13zm-2-7h2v2h5a1 1 0 0 1 1 1v4h-2v-3H10v10h4v2H9a1 1 0 0 1-1-1v-5H6v-2h2V9a1 1 0 0 1 1-1h5V6zM4 14v2H2v-2h2zm0-4v2H2v-2h2zm0-4v2H2V6h2zm0-4v2H2V2h2zm4 0v2H6V2h2zm4 0v2h-2V2h2zm4 0v2h-2V2h2z">
            </path>
          </g>
        </g>
      </svg>
      <span>Arrastra tu imagen</span>
    </template>
  </div>
</template>

<style scoped>
.image-container {
  border-radius: 10px;
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.3);
  color: #333;
}

.container_images {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  padding: 0;
  list-style: none;
}

.container_images li {
  text-align: center;
  width: 150px;
}

.container_images .image-wrapper {
  position: relative;
  width: 100%;
  padding-bottom: 100%;
  overflow: hidden;
}

.container_images .image-wrapper img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: contain;
  border-radius: 10px;
}

.container_images li span {
  display: block;
  margin-top: 10px;
  font-size: 14px;
}
</style>
