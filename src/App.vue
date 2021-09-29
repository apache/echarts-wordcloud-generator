<template>
<el-container>
    <el-aside>
        <h3>
            {{$t('title')}}
        </h3>
        <el-tabs type="card" v-model="activeName">
            <el-tab-pane label="样式" name="config">
                <WConfig></WConfig>
            </el-tab-pane>
            <el-tab-pane label="数据" name="data">数据</el-tab-pane>
            <el-tab-pane label="导出" name="export">导出</el-tab-pane>
        </el-tabs>
    </el-aside>
    <el-main>
        <WChart></WChart>
    </el-main>
</el-container>
</template>

<script lang='ts' setup>
import WChart from './components/WChart.vue';
import WConfig from './components/WConfig.vue';

const activeName = 'config';
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
}
</style>
