<template>
  <el-container>
    <el-aside>
      <h3>
        {{ $t('title') }}
      </h3>
      <el-tabs type="card" v-model="activeName">
        <el-tab-pane label="样式" name="config">
          <WConfig ref="wconfig" @change="onChange"></WConfig>
        </el-tab-pane>
        <el-tab-pane label="数据" name="data">
          <WData ref="wdata" @change="onChange"></WData>
        </el-tab-pane>
        <el-tab-pane label="导出" name="export">导出</el-tab-pane>
      </el-tabs>
    </el-aside>
    <el-main>
      <WChart ref="wchart"></WChart>
    </el-main>
  </el-container>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import { useI18n } from 'vue-i18n';
import WChart from './components/WChart.vue';
import WConfig from './components/WConfig.vue';
import WData from './components/WData.vue';
import defaultData from './data/defaultData';

const wconfig = ref<any>(null);
const wdata = ref<any>(null);
const wchart = ref<any>(null);

const { t } = useI18n({ useScope: 'global' });
const activeName = 'config';

function onChange() {
  wchart.value?.run(wdata.value?.data, wconfig.value?.getConfig());
}

setTimeout(() => {
  wdata.value?.setData(defaultData);
});
</script>

<style>
h4 {
  margin: 10px 0;
}
</style>

<style scoped lang="scss">
#echarts-spa-app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}

.el-aside {
  padding: 0 15px;
  --el-aside-width: 400px;
  height: calc(100vh - 50px);
  overflow: auto;
}
</style>
