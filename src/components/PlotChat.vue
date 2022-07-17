<script setup lang="ts">
import * as d3 from "d3"
import { onMounted, watchEffect, watch, toRefs, ref } from "vue";
import { getRandomCoordinates, getRandomColor } from "./RandomUtils"

const props = defineProps<{
    input: string
}>();

// init(input ?? "");

onMounted(() => {
    init(props.input ?? "");
})
const propsRef = toRefs(props);
const divRef = ref<HTMLDivElement | null>(null);

watch([propsRef.input], () => {
    console.log("new props.input", propsRef.input.value)
    init(propsRef.input.value ?? "");
}, { immediate: true })

function init(data: string) {
    const id = "plotChat"
    d3.select(`#${id}`).select("svg").remove();

    // const screenWidth = Math.min(document.documentElement.clientWidth, window.innerWidth || 0)
    // const width = screenWidth > 900 ? Math.ceil(screenWidth / 2) - 10 : screenWidth;
    const width = divRef.value?.clientWidth!;
    const height = 350;

    const margin = {
        top: 20,
        bottom: 20,
        left: 20,
        right: 20
    }

    const n = 100 + ((data.length + 1) * 50);
    const xThreshold = 250 + ((data.length % 5) * 5);
    const yThreshold = 250 + ((data.length % 10) * 10);

    let dataPoints = getRandomCoordinates(n, xThreshold, yThreshold);


    const xScale = d3.scaleLinear()
        .domain([0, xThreshold])
        .range([margin.left + margin.right, width - margin.left - margin.right]);
    const yScale = d3.scaleLinear()
        .domain([0, yThreshold])
        .range([height - margin.top - margin.bottom, margin.top]);

    const xAxis = d3.axisBottom(xScale);
    const yAxis = d3.axisLeft(yScale);

    let plotChartSVG = d3.select(`#${id}`)
        .append("svg")
        .attr("width", width)
        .attr("height", height);

    plotChartSVG.append("g")
        .selectAll("circle")
        .data(dataPoints)
        .enter()
        .append("circle")
        .attr("cx", d => xScale(d[0]))
        .attr("cy", d => yScale(d[1]))
        .attr("r", "7")
        .attr("fill", () => getRandomColor())
        .attr("fill-opacity", "0.7");

    plotChartSVG.append("g")
        .attr("transform", `translate(0, ${height - margin.top - margin.bottom})`)
        .attr("class", "axis")
        .call(xAxis);

    plotChartSVG.append("g")
        .attr("class", "axis")
        .attr("transform", `translate(${margin.left + margin.right} , 0)`)
        .call(yAxis);
}
</script>
<template>
    <div ref="divRef">
        <div id="plotChat"></div>
    </div>
</template>