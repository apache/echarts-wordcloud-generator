<template>
    <div>
        <div class="chart" ref="chartRef">aaa</div>
    </div>
</template>


<script setup lang="ts">
import { onMounted, shallowRef, ref } from 'vue';
import * as echarts from 'echarts';
import type { ECharts } from 'echarts';
import 'echarts-wordcloud';

// const props = defineProps({
//   foo: String
// });
const chart = shallowRef<ECharts | null>(null);
const chartRef = ref<HTMLElement | null>(null);

const run = () => {
    console.log('run', chart.value);
    chart.value!.setOption({
        series: [{
            type: 'wordCloud',
            data: [{
                name: 'series',
                value: 100
            }, {
                name: 'data',
                value: 30
            }, {
                name: 'itemStyle',
                value: 20
            }, {
                name: 'lineStyle',
                value: 5
            }]
        }]
    });
};

onMounted(() => {
    console.log('mounted', chartRef.value);
    chart.value = echarts.init(chartRef.value!);
    run();
});
</script>


<style scoped lang="scss">
.chart {
    border: 1px solid #f2eef2;
    width: 800px;
    height: 600px;
}
</style>
