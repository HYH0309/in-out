<template>
    <div class="file-viewer-container">
        <div class="line-selection-controls">
            <input type="number" v-model="startLine" @input="updateSelection" placeholder="Start Line" min="1"
                :max="maxLines" class="line-input" />
            <input type="number" v-model="endLine" @input="updateSelection" placeholder="End Line" min="1"
                :max="maxLines" class="line-input" />
            <select v-model="highlightColor" class="color-select">
                <option v-for="color in colors" :key="color" :value="color"
                    :style="`background-color: ${color}; color: ${contrastColor(color)};`">{{ color }}</option>
            </select>
            <div class="color-preview" :style="{ backgroundColor: highlightColor }"></div>
            <button @click="applyHighlight" class="toggle-button">Apply</button>
        </div>

        <div class="file-content-display">
            <div v-for="(line, index) in lines" :key="index" :class="lineClass(index)" class="line"
                :style="{ backgroundColor: toggledLines.includes(index) ? highlightColor : '' }">
                <span class="line-number">{{ index + 1 }}</span>
                {{ line }}
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, watch, computed } from 'vue';

const props = defineProps({
    content: String // 从父组件获取文件的内容
});

// 行内容初始化
const lines = ref((props.content || '').split('\n'));
const toggledLines = ref<number[]>([]);
const startLine = ref(1); // 默认起始行为1
const endLine = ref(1); // 默认结束行为1
const highlightColor = ref('#FFFF00'); // 默认高亮颜色为黄色
const colors = ref(['#FFCCCC', '#CCFFCC', '#CCCCFF', '#FFFFCC', '#FFCCFF']); // 预设颜色

// 计算最大行数
const maxLines = computed(() => lines.value.length);

// 更新行选择边界，确保行号在有效范围内
const updateSelection = () => {
    startLine.value = Math.max(1, Math.min(startLine.value, maxLines.value));
    endLine.value = Math.max(1, Math.min(endLine.value, maxLines.value));
};

// 应用高亮颜色
const applyHighlight = () => {
    toggleLines();
};

// 切换行的高亮状态
const toggleLines = () => {
    const start = Math.max(0, startLine.value - 1);
    const end = Math.min(maxLines.value - 1, endLine.value - 1);

    for (let i = start; i <= end; i++) {
        if (!toggledLines.value.includes(i)) {
            toggledLines.value.push(i);
        } else {
            toggledLines.value = toggledLines.value.filter(line => line !== i); // 若重新点击则取消高亮
        }
    }
};

// 监测 content 的变化以更新 lines
watch(() => props.content, newContent => {
    lines.value = (newContent || '').split('\n');
    // Reset toggledLines after content change
    toggledLines.value = [];
});

// 生成高亮行的 CSS 类名
const lineClass = (index: number) => {
    return {
        'customHighlight': toggledLines.value.includes(index)
    };
};

// 计算对比颜色用于增强可读性
const contrastColor = (color: string) => {
    const rgb = parseInt(color.slice(1), 16);
    const r = (rgb >> 16) & 0xff;
    const g = (rgb >> 8) & 0xff;
    const b = (rgb >> 0) & 0xff;
    const brightness = (r * 0.299 + g * 0.587 + b * 0.114);
    return brightness > 186 ? '#000000' : '#FFFFFF'; // 返回黑色或白色
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

.color-select,
.color-select option {
    @apply border border-gray-300 rounded p-2 mr-2;
}

.color-preview {
    @apply border rounded p-2 w-5 h-5 mr-10;
}

.toggle-button {
    @apply bg-white-500 text-black px-4 py-2 rounded hover:bg-gray-500;
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
    /* 这个类可以知道高亮的背景，但目前我们通过内联样式来设置颜色 */
}
</style>
