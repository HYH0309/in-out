<template>
    <div class="file-uploader-container">
        <input type="file" @change="handleFileUpload" accept=".in,.out" class="file-input" />
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['fileLoaded'])
const fileContent = ref("")

const handleFileUpload = async (event: Event) => {
    const target = event.target as HTMLInputElement;

    if (target.files && target.files.length) {
        const file = target.files[0];
        const reader = new FileReader();

        reader.onload = (e) => {
            // 将文件内容传递给父组件
            fileContent.value = e.target?.result as string;
            emit('fileLoaded', fileContent.value);
        };

        reader.readAsText(file);
    }
};
</script>

<style scoped>
.file-uploader-container {
    @apply p-4;
}

.file-input {
    @apply bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 focus:outline-none;
}
</style>
