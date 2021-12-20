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

type Config = {
  hue: number[];
  saturation: number[];
  lightness: number[];
  alpha: number[];
  fontFamily: string;
  fontSize: number[];
  rotate: number[];
  width: number;
  height: number;
  shape: string;
};

function run(data?: [], config?: Config) {
  config && console.log(config.width, config.height);
  chart.value!.setOption({
    series: [
      {
        type: 'wordCloud',
        data: data || [],
        // gridSize: 0,
        sizeRange: config?.fontSize,
        rotationRange: config?.rotate,
        shape: config?.shape,
        width: config?.width + '%',
        height: config?.height + '%',
        layoutAnimation: true,
        keepAspect: true
      }
    ],
    textStyle: {
      fontFamily: config?.fontFamily
    }
  });
}

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
