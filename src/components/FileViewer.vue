<template>
    <div class="file-viewer-container">
        <div class="line-selection-controls">
            <input type="number" v-model="startLine" @input="updateSelection" placeholder="Start Line" min="1"
                :max="maxLines" class="line-input" />
            <input type="number" v-model="endLine" @input="updateSelection" placeholder="End Line" min="1"
                :max="maxLines" class="line-input" />
            <select v-model="highlightColor" class="color-select">
                <option v-for="color in colors" :key="color" :value="color">{{ color }}</option>
            </select>
            <input type="color" v-model="customColor" class="custom-color-input" @input="updateCustomColor" />
            <button @click="applyHighlight" class="toggle-button">Apply Color</button>
        </div>
        <div class="file-content-display">
            <div v-for="(line, index) in lines" :key="index" :class="lineClass(index)" class="line">
                <span class="line-number">{{ index + 1 }}</span>
                {{ line }}
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps({
    content: String // 从父组件获取文件的内容
});

const lines = ref((props.content || '').split('\n'));
const toggledLines = ref<number[]>([]);
const startLine = ref<number | null>(null);
const endLine = ref<number | null>(null);
const maxLines = lines.value.length;
const highlightColor = ref('#FFFF00'); // 默认高亮颜色为黄色
const customColor = ref('#FFFF00'); // 用户自定义颜色
const colors = ref(['#FF0000', '#00FF00', '#0000FF', '#FFFF00', '#FF00FF']); // 预设颜色

const lineClass = (index: number) => {
    return {
        customHighlight: toggledLines.value.includes(index),
    };
};

// 更新行选择
const updateSelection = () => {
    // 确保行号在有效范围内
    if (startLine.value !== null && endLine.value !== null) {
        startLine.value = Math.max(1, Math.min(startLine.value, maxLines));
        endLine.value = Math.max(1, Math.min(endLine.value, maxLines));
    }
};

// 应用高亮颜色
const applyHighlight = () => {
    document.documentElement.style.setProperty('--highlight-color', highlightColor.value); // 设置 CSS 变量
    if (startLine.value !== null && endLine.value !== null) {
        const start = Math.min(startLine.value - 1, endLine.value - 1);
        const end = Math.max(startLine.value - 1, endLine.value - 1);

        for (let i = start; i <= end; i++) {
            if (!toggledLines.value.includes(i)) {
                toggledLines.value.push(i);
            }
        }
    }
};

// 更新自定义颜色
const updateCustomColor = () => {
    highlightColor.value = customColor.value; // 更新 highlightColor 为自定义颜色
};
</script>

<style scoped>
.file-viewer-container {
    @apply h-xl w-xl flex-1 p-4 box-border shadow-md rounded-lg border border-gray-300;
}

.line-selection-controls {
    @apply mb-4 flex items-center;
}

.line-input {
    @apply border border-gray-300 rounded p-2 mr-2 w-24;
}

.color-select {
    @apply border border-gray-300 rounded p-2 mr-2;
}

.custom-color-input {
    @apply border border-gray-300 rounded p-2 mr-2;
}

.toggle-button {
    @apply bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600;
}

.file-content-display {
    @apply w-full h-sm bg-white p-4 box-border outline-none rounded-lg;
    overflow-y: auto;
}

.line {
    @apply cursor-pointer;
}

.line-number {
    @apply text-gray-500 mr-4;
}

.customHighlight {
    background-color: var(--highlight-color);
    /* 使用 CSS 变量设置自定义高亮颜色 */
}
</style>
