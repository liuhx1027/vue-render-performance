<script setup lang="ts">
import { toRefs, computed } from "vue";
import { getRandomNumberBetween } from "./RandomUtils"

const props = defineProps<{
    input: string
}>();

const propsRef = toRefs(props);

const verticalLineGap = 150;
const verticalLines = [
    { text: "5", x: verticalLineGap },
    { text: "10", x: verticalLineGap * 2 },
    { text: "15", x: verticalLineGap * 3 },
    { text: "20", x: verticalLineGap * 4 },
];
const horizontalLines = [
    { text: "50", y: 296 },
    { text: "100", y: 236 },
    { text: "150", y: 175 },
    { text: "200", y: 115 },
    { text: "250", y: 54 },
];

const dStrings = computed(() => {
    const numberOfLines = (propsRef.input.value.length + 1) * 10;
    const width = 700 / numberOfLines;
    const points0 = new Array(numberOfLines).fill(0).map((_, idx) => {
        return { x: 50 + idx * width, y: 350 }
    });
    points0.push({ x: 750, y: 350 })

    const points1 = points0.map((p => ({ x: p.x, y: getRandomNumberBetween(300, p.y) })));
    const dString1 = points1.reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, "M 50 350") + " L 750 350 Z";

    const points2 = points1.map((p => ({ x: p.x, y: getRandomNumberBetween(200, p.y) })));
    const dString2 = points2.reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, `M 50 ${points1[0].y}`)
        + points1.reverse().reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, "")
        + " Z";

    const points3 = points2.map((p => ({ x: p.x, y: getRandomNumberBetween(100, p.y) })));
    const dString3 = points3.reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, `M 50 ${points2[0].y}`)
        + points2.reverse().reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, "")
        + " Z";

    const points4 = points3.map((p => ({ x: p.x, y: getRandomNumberBetween(50, p.y) })));
    const dString4 = points4.reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, `M 50 ${points3[0].y}`)
        + points3.reverse().reduce((pre, cur) => `${pre} L ${cur.x} ${cur.y}`, "")
        + " Z";

    return { dString1, dString2, dString3, dString4 }
})
</script>
<template>
    <div>
        <div id="areaChat">
            <svg width="800" height="400" role="img"
                aria-labelledby="victory-container-1-title victory-container-1-desc" viewBox="0 0 800 400"
                style="pointer-events: all; width: 100%; height: 100%;">
                <g role="presentation">
                    <line x1="50" x2="750" y1="350" y2="350" role="presentation" shape-rendering="auto"
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
                        <line x1="50" x2="750" :y1="line.y" :y2="line.y" role="presentation" shape-rendering="auto"
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
                    <path :d="dStrings.dString1" fill="#b3d1ff" stroke="#0074d9" />
                    <path :d="dStrings.dString2" fill="#00d1ff" stroke="#0074d9" />
                    <path :d="dStrings.dString3" fill="red" stroke="#0074d9" />
                    <path :d="dStrings.dString4" fill="blue" stroke="#0074d9" />
                </g>
            </svg>
        </div>
    </div>
</template>