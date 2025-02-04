<template>
  <div class="app-container">
    <div class="file-views">
      <div class="in-out-container">
        <div class="file-viewer-container">
          <div class="file-uploader-section">
            <FileUploader @fileLoaded="handleInputFileLoad" />
          </div>
          <FileViewer :content="inputContent" @updateOutput="updateInput" />
        </div>
        <div class="file-output-container">
          <div class="file-uploader-section">
            <FileUploader @fileLoaded="handleOutputFileLoad" />
          </div>
          <FileViewer :content="outputContent" @updateOutput="updateOutput" />
        </div>
      </div>
    </div>

    <div v-if="!inputContent && !outputContent" class="default-message">
      <p>请上传输入和输出文件以查看内容。</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import FileViewer from './components/FileViewer.vue';
import FileUploader from './components/FileUploader.vue';

const inputContent = ref('请上传输入文件。');  // 默认输入内容提示
const outputContent = ref('请上传输出文件。'); // 默认输出内容提示

// 处理输入文件的加载
function handleInputFileLoad(fileContent: string) {
  inputContent.value = fileContent;
}

// 处理输出文件的加载
function handleOutputFileLoad(fileContent: string) {
  outputContent.value = fileContent;
}

// 更新输入内容
function updateInput(newInput: string) {
  inputContent.value = newInput;
}

// 更新输出内容
function updateOutput(newOutput: string) {
  outputContent.value = newOutput;
}
</script>

<style>
.app-container {
  @apply flex flex-col items-center p-6 min-h-screen bg-gray-100;
}

.file-uploader-section {
  @apply mb-4 w-full max-w-md;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.file-uploader-section:hover {
  background-color: #f0f0f0;
}

.in-out-container {
  @apply flex flex-row space-x-4;
}

.file-viewer-container,
.file-output-container {
  @apply w-full max-w-md bg-white p-4 shadow-lg rounded-lg border border-gray-300;
}

.default-message {
  @apply mt-4 p-4 text-gray-600 rounded bg-gray-50 border border-gray-200;
}
</style>
