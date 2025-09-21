<template>
    <div class="slidev-layout default">
        <div class="flex items-center gap-5 mb-8">
            <img :src="logoUrl" alt="Lab banner" class="h-16 w-auto rounded-lg" />
            <div class="flex flex-col gap-1 text-left">
                <div class="text-3xl">{{ titleText }}</div>
                <div class="text-xs text-[#5b6b82] tracking-wide">{{ topic }}</div>
            </div>
        </div>

        <div class="my-auto">
            <component v-for="(node, i) in filteredNodes" :is="node" :key="i" />
        </div>
    </div>
</template>

<script setup lang="ts">
import { useSlideContext } from '@slidev/client'
import { ref, onMounted, useSlots } from 'vue'

const { $slidev } = useSlideContext();
const configs = $slidev.configs as any;
const logoUrl = configs.logoUrl || 'https://picx.zhimg.com/80/v2-32c67121f7b3b7395aec3ad20fb97713_1440w.png';
const topic = configs.topic || '';

// 获取默认 slot 的内容
const slots = useSlots()
const slotNodes = slots.default?.() || []

// 提取第一个 h1 的文字
const firstH1Node = slotNodes.find(node => node.type === 'h1')
const titleText = firstH1Node?.children || 'Table of Contents'

// 过滤掉第一个 h1，保留其余节点
const filteredNodes = slotNodes.filter(node => node !== firstH1Node)
</script>
