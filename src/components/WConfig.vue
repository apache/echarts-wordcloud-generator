<template>
  <el-collapse>
    <el-collapse-item title="颜色" name="color">
      <!-- <h5>基础色</h5>
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

      <h5>色相范围</h5>
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

      <h5>饱和度范围</h5>
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

      <h5>亮度范围</h5>
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

      <h5>透明度范围</h5>
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
      <el-row>
        <el-col :span="12">
          <h5>字体</h5>
        </el-col>
        <el-col :span="12">
          <el-select
            v-model="selectedFontFamily"
            placeholder="请选择字体"
            @change="changeFontFamily"
          >
            <el-option
              v-for="item in fontFamilies"
              :key="item"
              :label="item"
              :value="item"
            >
            </el-option>
          </el-select>
        </el-col>
      </el-row>

      <h5>字号范围</h5>
      <el-row>
        <el-col :span="22" :offset="1">
          <el-slider
            v-model="fontSize"
            range
            show-tooltip
            :max="100"
            :step="1"
            input-size="medium"
            @change="change"
          >
          </el-slider>
        </el-col>
      </el-row>

      <h5>旋转范围</h5>
      <el-row>
        <el-col :span="22" :offset="1">
          <el-slider
            v-model="rotate"
            range
            show-tooltip
            :min="-180"
            :max="180"
            :step="45"
            input-size="medium"
            @change="change"
          >
          </el-slider>
        </el-col>
      </el-row>
    </el-collapse-item>

    <el-collapse-item title="形状" name="mask">
      <h5>宽度（百分比）</h5>
      <el-row>
        <el-col :span="22" :offset="1">
          <el-slider
            v-model="width"
            show-tooltip
            :max="100"
            :step="5"
            input-size="medium"
            @change="change"
          >
          </el-slider>
        </el-col>
      </el-row>

      <h5>高度（百分比）</h5>
      <el-row>
        <el-col :span="22" :offset="1">
          <el-slider
            v-model="height"
            show-tooltip
            :max="100"
            :step="5"
            input-size="medium"
            @change="change"
          >
          </el-slider>
        </el-col>
      </el-row>

      <el-row>
        <el-col :span="12">
          <h5>遮罩形状</h5>
        </el-col>
        <el-col :span="12">
          <el-select
            v-model="selectedMask"
            placeholder="请选择遮罩图形"
            @change="changeMask"
          >
            <el-option
              v-for="item in masks"
              :key="item.value"
              :label="item.name"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </el-col>
      </el-row>
    </el-collapse-item>
  </el-collapse>
</template>

<script setup lang="ts">
import { ref } from "vue";
// const themeColors = ref(['#720FEB', '#EB1AA9', '#B6DA02']);
const hue = ref([0, 255]);
const saturation = ref([0.5, 0.8]);
const lightness = ref([0.5, 0.8]);
const alpha = ref([0.5, 0.8]);
const selectedFontFamily = ref("Arial");
const fontSize = ref([4, 100]);
const rotate = ref([-90, 90]);
const width = ref(90);
const height = ref(90);
const selectedMask = ref("circle");

const fontFamilies = [
  "Arial",
  "Lato",
  "Times New Roman",
  "Courier New",
  "Georgia",
  "Helvetica",
  "Lucida Sans",
  "Tahoma",
  "Verdana"
];
const masks = [
  {
    name: "椭圆",
    value: "circle"
  },
  {
    name: "方形",
    value: "square"
  },
  {
    name: "菱形",
    value: "diamond"
  },
  {
    name: "三角形",
    value: "triangle"
  },
  {
    name: "五边形",
    value: "pentagon"
  },
  {
    name: "五角星",
    value: "star"
  },
  {
    name: "爱心",
    value: "cardioid"
    // }, {
    //     name: '自定义图片',
    //     value: 'image'
  }
];

const emit = defineEmits(["change"]);

defineExpose({ getConfig });

function changeFontFamily() {
  change();
}

function changeMask() {
  change();
}

function change() {
  emit("change");
}

function getConfig() {
  return {
    hue: hue.value,
    saturation: saturation.value,
    lightness: lightness.value,
    alpha: alpha.value,
    fontFamily: selectedFontFamily.value,
    fontSize: fontSize.value,
    rotate: rotate.value,
    width: width.value,
    height: height.value,
    shape: selectedMask.value === "image" ? null : selectedMask.value
  };
}
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
  color: #409eff;
}

.text-pad {
  padding: 8px 0;
}

.el-dropdown-link {
  cursor: pointer;
  color: #409eff;
}

.el-icon-arrow-down {
  font-size: 12px;
}

h5 {
  margin: 8px 0;
  font-size: 13px;
  color: #666;
}
</style>
