<template>
    <h4>主题色</h4>
    <div>
        <el-color-picker
            v-for="(color, index) in themeColors"
            v-bind:key="index"
            v-model="themeColors[index]"
            size="small"
        >
        </el-color-picker>
        <div class="color-picker-btn">
            <i class="el-icon-minus"></i>
        </div>
        <div class="color-picker-btn">
            <i class="el-icon-plus"></i>
        </div>
    </div>
    <h4>亮度范围</h4>
    <el-row>
        <el-col :span="22" :offset="1">
            <el-slider
            v-model="value"
            range
            show-tooltip
            :max="1"
            :step="0.05"
            input-size="medium"
            >
            </el-slider>
        </el-col>
    </el-row>

    <h4>饱和度范围</h4>
    <el-row>
        <el-col :span="22" :offset="1">
            <el-slider
            v-model="value"
            range
            show-tooltip
            :max="1"
            :step="0.05"
            input-size="medium"
            >
            </el-slider>
        </el-col>
    </el-row>

    <h4>透明度范围</h4>
    <el-row>
        <el-col :span="22" :offset="1">
            <el-slider
            v-model="value"
            range
            show-tooltip
            :max="1"
            :step="0.05"
            input-size="medium"
            >
            </el-slider>
        </el-col>
    </el-row>
    <!-- <el-collapse>
        <el-collapse-item title="一致性 Consistency" name="1">
        </el-collapse-item>
    </el-collapse> -->
</template>


<script setup lang="ts">
const themeColors = ['#720FEB', '#EB1AA9', '#EB1AA2'];
</script>

<style lang="scss">
.title-right {
    position: absolute;
    top: 10px;
    right: 0;

    a {
        display: inline-block;
    }
}

.el-color-picker {
    margin-right: 5px;
}

.color-picker-btn {
    display: inline-block;
    width: 30px;
    height: 30px;
    margin-right: 5px;
    padding: 3px 6px;
    box-sizing: border-box;
    vertical-align: top;
    border: 1px solid #e6e6e6;
    border-radius: 4px;
    color: #409EFF;
}

.text-pad {
    padding: 8px 0;
}
</style>
