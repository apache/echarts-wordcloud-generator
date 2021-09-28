<template>
<el-container>
    <el-aside>
        <h3>
            {{$t('title')}}
        </h3>
    </el-aside>
    <el-main>
        <div id="chart"></div>
    </el-main>
</el-container>
</template>

<script lang='ts' setup>

</script>

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

#chart {
    border: 1px solid #f2eef2;
    width: 800px;
    height: 600px;
}
</style>
