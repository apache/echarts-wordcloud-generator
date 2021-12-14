<template>
    <div>
        <div class="chart" ref="chartRef">aaa</div>
    </div>
</template>


<script setup lang="ts">
import { onMounted, shallowRef, ref } from 'vue';
import * as echarts from 'echarts';
import 'echarts-wordcloud';

// const props = defineProps({
//   foo: String
// });
const chart = shallowRef<echarts.ECharts | null>(null);
const chartRef = ref<HTMLElement | null>(null);

defineExpose({
    run
});

function run(data?: []) {
    chart.value!.setOption({
        series: [{
            type: 'wordCloud',
            data: data || []
        }]
    });
};

onMounted(() => {
    chart.value = echarts.init(chartRef.value!);
});
</script>


<style scoped lang="scss">
.chart {
    border: 1px solid #f2eef2;
    width: 800px;
    height: 600px;
}
</style>
