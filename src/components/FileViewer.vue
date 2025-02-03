<template>
    <div class="file-viewer-container">
        <textarea readonly :value="content" class="file-content-display"></textarea>
    </div>
</template>

<script setup lang="ts">
defineProps({
    content: String // 从父组件获取文件的内容
});
</script>

<style scoped>
.file-viewer-container {
    @apply flex-1 p-4 box-border shadow-md rounded-lg border border-gray-300;
}

.file-content-display {
    @apply w-full h-full bg-white text-black p-4 box-border outline-none rounded-lg;
}
</style>
