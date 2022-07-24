<script setup lang="ts">
import * as d3 from "d3"
import { onMounted, watchEffect, watch, toRefs, ref, computed } from "vue";
import { getRandomCoordinates, getRandomColor } from "./RandomUtils"

const props = defineProps<{
    input: string
}>();

const propsRef = toRefs(props);
const divRef = ref<HTMLDivElement | null>(null);

watch([propsRef.input], () => {
}, { immediate: true })

const verticalLines = [
    { text: "5", x: 112.5 },
    { text: "10", x: 175 },
    { text: "15", x: 237.5 },
    { text: "20", x: 300 },
];
const horizontalLines = [
    { text: "50", y: 296 },
    { text: "100", y: 236 },
    { text: "150", y: 175 },
    { text: "200", y: 115 },
    { text: "250", y: 54 },
];

function randomIntFromInterval(min: number, max: number) { // min and max included 
    return Math.floor(Math.random() * (max - min + 1) + min)
}

const plots = computed(() => new Array((propsRef.input.value.length + 1) * 20).fill(0).map(() => {
    return { x: randomIntFromInterval(50, 350), y: randomIntFromInterval(50, 350), color: getRandomColor() };
}));
</script>
<template>
    <div ref="divRef">
        <div id="plotChat">
            <svg width="400" height="400" role="img"
                aria-labelledby="victory-container-1-title victory-container-1-desc" viewBox="0 0 400 400"
                style="pointer-events: all; width: 100%; height: 100%;">
                <g role="presentation">
                    <line x1="50" x2="350" y1="350" y2="350" role="presentation" shape-rendering="auto"
                        vector-effect="non-scaling-stroke"
                        style="stroke: white; fill: transparent; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round;">
                    </line>
                    <g role="presentation" v-for="line in verticalLines">
                        <line :x1="line.x" :x2="line.x" y1="350" y2="50" role="presentation" shape-rendering="auto"
                            vector-effect="non-scaling-stroke"
                            style="stroke: rgb(236, 239, 241); fill: none; stroke-dasharray: 10, 5; stroke-linecap: round; stroke-linejoin: round; pointer-events: painted;">
                        </line>
                        <line :x1="line.x" :x2="line.x" y1="350" y2="355" role="presentation" shape-rendering="auto"
                            vector-effect="non-scaling-stroke"
                            style="stroke: rgb(144, 164, 174); fill: transparent; size: 5px; stroke-width: 1; stroke-linecap: round; stroke-linejoin: round;">
                        </line><text :x="line.x" dx="0" y="363" dy="10.26">
                            <tspan :x="line.x" dx="0" text-anchor="middle"
                                style="fill: white; font-size: 12px; font-family: Roboto, &quot;Helvetica Neue&quot;, Helvetica, sans-serif; stroke: transparent; letter-spacing: normal; padding: 8px; stroke-width: 0;">
                                {{ line.text }}</tspan>
                        </text>
                    </g>
                </g>
                <g role="presentation">
                    <line x1="50" x2="50" y1="50" y2="350" role="presentation" shape-rendering="auto"
                        vector-effect="non-scaling-stroke"
                        style="stroke: white; fill: transparent; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round;">
                    </line>
                    <g role="presentation" v-for="line in horizontalLines">
                        <line x1="50" x2="350" :y1="line.y" :y2="line.y" role="presentation" shape-rendering="auto"
                            vector-effect="non-scaling-stroke"
                            style="stroke: rgb(236, 239, 241); fill: none; stroke-dasharray: 10, 5; stroke-linecap: round; stroke-linejoin: round; pointer-events: painted;">
                        </line>
                        <line x1="50" x2="45" :y1="line.y" :y2="line.y" role="presentation" shape-rendering="auto"
                            vector-effect="non-scaling-stroke"
                            style="stroke: rgb(144, 164, 174); fill: transparent; size: 5px; stroke-width: 1; stroke-linecap: round; stroke-linejoin: round;">
                        </line><text x="37" dx="0" :y="line.y" dy="4.26">
                            <tspan x="37" dx="0" text-anchor="end"
                                style="fill: white; font-size: 12px; font-family: Roboto, &quot;Helvetica Neue&quot;, Helvetica, sans-serif; stroke: transparent; letter-spacing: normal; padding: 8px; stroke-width: 0;">
                                {{ line.text }}</tspan>
                        </text>
                    </g>
                </g>
                <g>
                    <path v-for="p in plots" :d="`M ${p.x}, ${p.y}
                    m -6, 0
                    a 6, 6 0 1,0 12,0
                    a 6, 6 0 1,0 -12,0`" role="presentation" shape-rendering="auto"
                        :style="`fill: ${p.color}; opacity: 1; stroke: transparent; stroke-width: 0;`"></path>
                </g>
            </svg>
        </div>
    </div>
</template>