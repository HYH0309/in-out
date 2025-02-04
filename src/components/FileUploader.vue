<template>
    <div class="file-uploader-container">
        <input type="file" id="fileInput" @change="handleFileUpload" accept=".in,.out" class="file-input" />
        <label for="fileInput" class="custom-file-input">
            选择文件
        </label>
    </div>
</template>

<script setup lang="ts">
import { defineEmits } from 'vue';

const emit = defineEmits(['fileLoaded']);

const handleFileUpload = async (event: Event) => {
    const target = event.target as HTMLInputElement;

    if (target.files && target.files.length) {
        const file = target.files[0];
        const reader = new FileReader();

        reader.onload = (e) => {
            emit('fileLoaded', e.target?.result as string);
        };

        reader.readAsText(file);
    }
};
</script>

<style scoped>
.file-uploader-container {
    @apply p-4 flex justify-center;
    position: relative;
    /* 用于定位 label */
}

.file-input {
    @apply absolute top-0 left-0 opacity-0 bg-black;
    /* 隐藏文件输入框 */
    width: 100%;
    /* 充满父元素 */
    height: 100%;
    /* 充满父元素 */
}


.custom-file-input {
    @apply bg-white border rounded-lg shadow-md p-2 text-gray-700 cursor-pointer hover:bg-gray-100 transition duration-200;
}
</style>
