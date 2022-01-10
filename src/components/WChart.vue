<template>
  <div v-loading="isWebFontLoading" element-loading-text="字体加载中">
    <div
      class="chart"
      ref="chartRef"
      v-bind:style="{
        width: width + 'px',
        height: height + 'px'
      }"
    ></div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, shallowRef, ref } from 'vue';
import * as echarts from 'echarts';
import 'echarts-wordcloud';
import Color from 'color';

const chart = shallowRef<echarts.ECharts | null>(null);
const chartRef = ref<HTMLElement | null>(null);
const isWebFontLoading = ref(false);
const width = ref(800);
const height = ref(600);

defineExpose({
  run,
  setLoading,
  resize,
  download
});

type Config = {
  bgColor: string;
  themeColors: string[];
  saturation: number[];
  lightness: number[];
  alpha: number[];
  fontFamily: string;
  fontSize: number[];
  rotate: number[];
  width: number;
  height: number;
  shape: string;
  shapeRatio: boolean;
};

function setLoading(isLoading: boolean) {
  isWebFontLoading.value = isLoading;
}

function resize(w: number, h: number) {
  w = Math.max(100, w);
  h = Math.max(100, h);
  if (width.value !== w || height.value !== h) {
    width.value = w;
    height.value = h;
  }
  setTimeout(() => {
    chart.value!.resize();
  });
}

function download() {
  if (chart.value) {
    try {
      const url = chart.value.getDataURL();
      const a = document.createElement('a');
      a.href = url;
      a.download = 'wordcloud.png';
      a.click();
    } catch (e) {
      console.error(e);
      alert('保存出错了，可以尝试在右图中点击鼠标右键，并选择“图片储存为”');
    }
  }
}

function run(data?: [], fillSmall?: boolean, config?: Config) {
  const chartData = (data ? data.slice() : []) as {
    name: string;
    value: number;
  }[];
  // if (fillSmall) {
  //   const smallData = chartData.filter((d) => d.value < 10);
  //   const maxData = 400;
  //   for (let i = chartData.length; i < maxData; ++i) {
  //     for (let j = 0; j < smallData.length && i < maxData; ++j) {
  //       chartData.push({
  //         name: smallData[j].name,
  //         value: smallData[j].value
  //       });
  //     }
  //   }
  // }

  const hues = config
    ? config.themeColors.map((color) => Color(color).hsl().object().h)
    : [];

  function getHue() {
    const index = Math.floor(Math.random() * hues.length);
    return hues[index];
  }

  function getRandom(minMax: number[] | undefined) {
    if (!minMax) {
      return 0;
    }
    const max = minMax[1] == null ? 1 : minMax[1];
    const min = minMax[0] == null ? 0 : minMax[0];
    const range = max - min || 1;
    return Math.random() * range + min;
  }

  function render(maskImage?: HTMLImageElement) {
    chart.value!.setOption({
      backgroundColor: config!.bgColor,
      series: [
        {
          type: 'wordCloud',
          data: chartData,
          gridSize: 4,
          sizeRange: config?.fontSize,
          rotationRange: config?.rotate,
          maskImage,
          width: config?.width + '%',
          height: config?.height + '%',
          layoutAnimation: true,
          keepAspect: config?.shapeRatio,
          textStyle: {
            color: (param: any) => {
              const value = param.value;
              const h = getHue();
              const s = getRandom(config?.saturation);
              const l = getRandom(config?.lightness);
              const color = Color(`hsl(${h}, ${s}%, ${l}%)`);
              return color.toString();
            }
          }
        }
      ],
      textStyle: {
        fontFamily: config?.fontFamily
      }
    });
  }

  let maskImage: HTMLImageElement;
  if (config) {
    maskImage = new Image();
    maskImage.src = config.shape.startsWith('blob:')
      ? config.shape
      : config.shape + '.png';
    maskImage.onload = () => {
      render(maskImage);
    };
    maskImage.onerror = () => {
      render();
    };
  } else {
    render();
  }
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
