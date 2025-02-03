<template>
  <div class="app-container">
    <h1 class="header-title">文件内容查看器</h1>
    <FileUploader @fileLoaded="handleFileLoad" />

    <FileViewer :content="content" v-if="content" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import FileViewer from './components/FileViewer.vue'
import FileUploader from './components/FileUploader.vue'
const content = ref("")

function handleFileLoad(fileContent: string) {
  content.value = fileContent;
}
</script>

<style>
.app-container {
  @apply flex flex-col items-center p-6 min-h-screen bg-gray-100;
}

.header-title {
  @apply text-3xl font-bold mb-6 text-gray-800;
}

.load-button {
  @apply bg-blue-500 text-white font-medium px-6 py-3 rounded-lg shadow hover:bg-blue-600 transition duration-300 ease-in-out focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2;
}
</style>
