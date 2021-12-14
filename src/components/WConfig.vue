<template>
    <el-collapse>
        <el-collapse-item title="颜色" name="color">
            <!-- <h4>基础色</h4>
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
            </div> -->

            <h4>色相范围</h4>
            <el-row>
                <el-col :span="22" :offset="1">
                    <el-slider
                        v-model="hue"
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
                        v-model="saturation"
                        range
                        show-tooltip
                        :max="1"
                        :step="0.05"
                        input-size="medium"
                    >
                    </el-slider>
                </el-col>
            </el-row>

            <h4>亮度范围</h4>
            <el-row>
                <el-col :span="22" :offset="1">
                    <el-slider
                        v-model="lightness"
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
                        v-model="alpha"
                        range
                        show-tooltip
                        :max="1"
                        :step="0.05"
                        input-size="medium"
                    >
                    </el-slider>
                </el-col>
            </el-row>
        </el-collapse-item>

        <el-collapse-item title="文字" name="font">
            <!-- <h4>字体</h4> -->
            <el-row>
                <el-col :span="12">
                    <el-label>字体</el-label>
                </el-col>
                <el-col :span="12">
                </el-col>
            </el-row>

        </el-collapse-item>
    </el-collapse>
</template>


<script setup lang="ts">
import { ref } from 'vue'
const themeColors = ref(['#720FEB', '#EB1AA9', '#B6DA02']);
const hue = ref([0, 255]);
const saturation = ref([0.5, 0.8]);
const lightness = ref([0.5, 0.8]);
const alpha = ref([0.5, 0.8]);
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

.el-dropdown-link {
    cursor: pointer;
    color: #409EFF;
}

.el-icon-arrow-down {
    font-size: 12px;
}
</style>
