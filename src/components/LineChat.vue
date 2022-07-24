<script setup lang="ts">
import { toRefs, computed } from "vue";
import { getRandomColor, getRandomNumberBetween } from "./RandomUtils"

const props = defineProps<{
    input: string
}>();

const propsRef = toRefs(props);

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

const bars = computed(() => {
    const NUMBER_OF_BAR = (propsRef.input.value.length + 1) * 20;
    const BAR_AREA_WIDTH = 300 / NUMBER_OF_BAR;
    const GAP = BAR_AREA_WIDTH * 0.2;
    const BAR_WIDTH = BAR_AREA_WIDTH - GAP;
    return new Array(NUMBER_OF_BAR).fill(0).map((_, idx) => {
        const height = getRandomNumberBetween(0, 300);
        return {
            x: 50 + idx * BAR_AREA_WIDTH,
            y: 350 - height,
            width: BAR_WIDTH,
            height: height
        }
    })
}) 
</script>
<template>
    <div>
        <div id="lineChat">
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
                    <rect v-for="(bar, index) in bars" :key="index" :x="bar.x" :y="bar.y" :width="bar.width"
                        :height="bar.height" :fill="getRandomColor()"></rect>
                </g>
            </svg>
        </div>
    </div>
</template>