<template>
    <div class="chart" ref="chartRef"></div>
</template>


<script setup lang="ts">
import { onMounted, ref } from 'vue';
// const { proxy } = getCurrentInstance();
onMounted(() => {
    const chartRef = ref(null);
    console.log('mounted', chartRef);
});
</script>


<style scoped lang="scss">
.chart {
    border: 1px solid #f2eef2;
    width: 800px;
    height: 600px;
}
</style>
