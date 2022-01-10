<template>
  <el-container>
    <el-aside>
      <h3>
        {{ $t('title') }}
      </h3>
      <el-tabs type="card" v-model="activeName">
        <el-tab-pane id="config-tab" label="样式" name="config">
          <WConfig
            ref="wconfig"
            @change="onChange"
            @fontLoading="onFontLoading"
            @fontLoaded="onFontLoaded"
          >
          </WConfig>
        </el-tab-pane>
        <el-tab-pane label="数据" name="data">
          <WData ref="wdata" @change="onChange"></WData>
        </el-tab-pane>
        <el-tab-pane label="导出" name="export">
          <WExport @sizeChange="onSizeChange" @download="onDownload"></WExport>
        </el-tab-pane>
        <el-tab-pane label="关于" name="about">
          <h5>项目说明</h5>
          <p class="small">
            <a
              href="https://github.com/ecomfe/echarts-wordcloud"
              target="_blank"
              >echarts-wordcloud</a
            >
            是
            <a href="https://github.com/apache/echarts" target="_blank"
              >Apache ECharts</a
            >
            基于
            <a href="https://github.com/timdream/wordcloud2.js" target="_blank"
              >wordcloud2.js</a
            >
            的插件，需要使用 JavaScript
            开发。本项目是基于该插件的字符云生成器，用于方便用户无代码生成字符云效果。如果需要更丰富的效果，可以考虑基于上述字符云插件开发。
          </p>
          <h5>项目源码</h5>
          <p class="small">
            <a
              href="https://github.com/apache/echarts-wordcloud-generator"
              target="_blank"
            >
              apache/echarts-wordcloud-generator
            </a>
          </p>
          <h5>版权说明</h5>
          <p class="small">
            从本工具下载的图片可免费个人使用或商业使用。源代码版权：
            <a
              href="https://github.com/apache/echarts-wordcloud-generator/blob/master/LICENSE"
              target="_blank"
            >
              Apache License 2.0</a
            >。
          </p>
        </el-tab-pane>
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
import WExport from './components/WExport.vue';
import defaultData from './data/defaultData';

const wconfig = ref<any>(null);
const wdata = ref<any>(null);
const wchart = ref<any>(null);

const { t } = useI18n({ useScope: 'global' });
const activeName = 'config';

function onChange() {
  wchart.value?.run(
    wdata.value?.data,
    wdata.value?.fillSmall,
    wconfig.value?.getConfig()
  );
}

function onSizeChange(size: { width: number; height: number }) {
  wchart.value?.resize(size.width, size.height);
}

function onDownload() {
  wchart.value?.download();
}

function onFontLoading() {
  wchart.value?.setLoading(true);
}

function onFontLoaded() {
  wchart.value?.setLoading(false);
}

setTimeout(() => {
  wdata.value?.setData(defaultData);
});
</script>

<style>
h4 {
  margin: 10px 0;
}

.el-collapse:first-child {
  border-top: 0;
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

#config-tab {
  margin-top: -15px;
}

.small {
  margin: 5px 0 20px 0;
  font-size: 14px;
}
</style>
