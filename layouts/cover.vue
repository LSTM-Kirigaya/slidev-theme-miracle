<template>
    <div class="slidev-layout cover relative text-white min-h-full font-sans">

        <!-- 顶部 Banner -->
        <div class="flex items-center gap-5">
            <img :src="logoUrl" alt="Lab banner" class="h-16 w-auto rounded-lg" />
            <div class="flex flex-col gap-1 text-left">
                <div class="text-3xl">{{ coverOrganization }}</div>
                <div class="text-xs text-[#5b6b82] tracking-wide">{{ topic }}</div>
            </div>
        </div>


        <!-- Title / Subtitle -->
        <div class="flex-1 flex flex-col gap-2 justify-center">
            <slot />
        </div>


        <!-- meta 信息 -->
        <div class="flex flex-wrap gap-3">
            <div v-if="coverAuthors.length"
                class="flex items-center gap-1 bg-white/5 backdrop-blur-sm rounded-lg px-3 py-2 text-sm border border-white/10">
                <template v-for="(coverAuthor, idx) in coverAuthors" :key="idx">
                    <mdi-account-circle />
                    <TextWithOptionalLink :link="coverAuthorUrls?.[idx]" :text="coverAuthor" />
                    <span v-if="idx < coverAuthors.length - 2">, </span>
                    <span v-if="idx === coverAuthors.length - 2"> 和 </span>
                </template>
            </div>

            <div v-if="coverDate"
                class="flex items-center gap-1 bg-white/5 backdrop-blur-sm rounded-lg px-3 py-2 text-sm border border-white/10">
                <mdi-calendar-clock />{{ coverDate }}
            </div>

            <div v-if="coverOccasion"
                class="flex items-center gap-1 bg-white/5 backdrop-blur-sm rounded-lg px-3 py-2 text-sm border border-white/10">
                <mdi-map-marker-radius />{{ coverOccasion }}
            </div>
        </div>
    </div>

</template>


<script setup lang="ts">
import { computed } from 'vue';
import { useSlideContext } from '@slidev/client';
import { handleBackground } from '../layout-helper';

const {
    coverAuthor: coverAuthorTransferred,
    coverAuthorUrl: coverAuthorUrlTransferred,
    coverBackgroundUrl,
    coverDate = new Date().toLocaleDateString(),
    logoUrl = 'https://picx.zhimg.com/80/v2-32c67121f7b3b7395aec3ad20fb97713_1440w.png',
    topic = 'paper reading',
    coverOrganization = 'Miracle Lab'
} = defineProps<{
    coverAuthor?: string | string[]
    coverAuthorUrl?: string | string[]
    coverBackgroundUrl?: string
    coverDate?: string | Date
    logoUrl?: string
    topic?: string
    coverOrganization?: string
    coverOccasion?: string
}>()

const transformIntoArray = (value: string | string[] | undefined) => {
    if (Array.isArray(value)) return value
    if (!value) return []
    return [value]
}

const coverAuthors = computed(() => transformIntoArray(coverAuthorTransferred))
const coverAuthorUrls = computed(() => transformIntoArray(coverAuthorUrlTransferred))

const baseStyle = `background: linear-gradient(135deg, #174891 0%, #0d2a52 100%);`

const style = computed(() => {
    const bg = handleBackground(coverBackgroundUrl, true)
    return `${baseStyle}${bg ? ';' + bg : ''}`
})
</script>
