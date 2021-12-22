<template>
  <el-collapse>
    <el-collapse-item title="颜色" name="color">
      <h5>配色方案</h5>
      <div>
        <div
          class="color-palette"
          v-for="palette in colorPalettes"
          :style="{ background: palette.bgColor }"
          @click="useColorPalette(palette)"
        >
          <div
            class="color"
            v-for="color in palette.themeColors"
            :style="{ background: color }"
          ></div>
        </div>
      </div>

      <h5>背景色</h5>
      <el-color-picker v-model="bgColor" size="small" @change="change">
      </el-color-picker>

      <h5>色相范围</h5>
      <el-row>
        <el-col :span="14">
          <el-color-picker
            v-for="(color, index) in themeColors"
            v-bind:key="index"
            v-model="themeColors[index]"
            size="small"
            @change="changeColor"
          >
          </el-color-picker>
          <div class="color-picker-btn" @click="removeThemeColor()">
            <i class="el-icon-minus"></i>
          </div>
          <div class="color-picker-btn" @click="addThemeColor()">
            <i class="el-icon-plus"></i>
          </div>
        </el-col>
        <el-col :span="8" :offset="2">
          <el-checkbox>关联数据大小</el-checkbox>
        </el-col>
      </el-row>

      <h5>饱和度范围</h5>
      <el-row>
        <el-col :span="13" :offset="1">
          <el-slider
            v-model="saturation"
            range
            show-tooltip
            :max="100"
            :step="1"
            input-size="medium"
            @change="change"
          >
          </el-slider>
        </el-col>
        <el-col :span="8" :offset="2">
          <el-checkbox>关联数据大小</el-checkbox>
        </el-col>
      </el-row>

      <h5>亮度范围</h5>
      <el-row>
        <el-col :span="13" :offset="1">
          <el-slider
            v-model="lightness"
            range
            show-tooltip
            :max="100"
            :step="1"
            input-size="medium"
            @change="change"
          >
          </el-slider>
        </el-col>
        <el-col :span="8" :offset="2">
          <el-checkbox>关联数据大小</el-checkbox>
        </el-col>
      </el-row>

      <!-- <h5>透明度范围</h5>
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
      </el-row> -->
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
            :max="200"
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
import { ref } from 'vue';
import Color from 'color';

const colorPalettes = [
  {
    bgColor: '#f8eddc',
    themeColors: ['#f19d70', '#b7b7a4', '#6b705c']
  },
  {
    bgColor: '#eef6fa',
    themeColors: ['#5470c6', '#91cc75', '#fac858', '#ee6666']
  },
  {
    bgColor: '#dbf3ff',
    themeColors: ['#4aa6d5', '#d3b16a', '#fb8500']
  },
  // {
  //   bgColor: '#eae2b7',
  //   themeColors: ['#003049', '#d62828', '#f77f00', '#fcbf49']
  // },
  {
    bgColor: '#fbffd1',
    themeColors: ['#132a13', '#90a955', '#ecf39e']
  }
];

const bgColor = ref(colorPalettes[0].bgColor);
const themeColors = ref(colorPalettes[0].themeColors);
const saturation = ref([50, 80]);
const lightness = ref([50, 80]);
const alpha = ref([0.5, 0.8]);
const selectedFontFamily = ref('Arial');
const fontSize = ref([4, 100]);
const rotate = ref([-90, 90]);
const width = ref(90);
const height = ref(90);
const selectedMask = ref('circle');

const fontFamilies = [
  'Arial',
  'Lato',
  'Times New Roman',
  'Courier New',
  'Georgia',
  'Helvetica',
  'Lucida Sans',
  'Tahoma',
  'Verdana'
];
const masks = [
  {
    name: '椭圆',
    value: 'circle'
  },
  {
    name: '方形',
    value: 'square'
  },
  {
    name: '菱形',
    value: 'diamond'
  },
  {
    name: '三角形',
    value: 'triangle'
  },
  {
    name: '五边形',
    value: 'pentagon'
  },
  {
    name: '五角星',
    value: 'star'
  },
  {
    name: '爱心',
    value: 'cardioid'
    // }, {
    //     name: '自定义图片',
    //     value: 'image'
  }
];

const emit = defineEmits(['change']);

defineExpose({ getConfig });

setTimeout(() => {
  changeColor();
}, 0);

function changeFontFamily() {
  change();
}

function changeMask() {
  change();
}

function changeColor() {
  let minS = 100;
  let maxS = 0;
  let minL = 100;
  let maxL = 0;
  themeColors.value.forEach(color => {
    const c = Color(color);
    const s = Math.round(c.saturationv());
    const l = Math.round(c.lightness());
    if (s < minS) {
      minS = s;
    }
    if (s > maxS) {
      maxS = s;
    }
    if (l < minL) {
      minL = l;
    }
    if (l > maxL) {
      maxL = l;
    }
  });
  saturation.value = [minS, maxS];
  lightness.value = [minL, maxL];
  change();
}

function change() {
  emit('change');
}

function useColorPalette(palette: { bgColor: string; themeColors: string[] }) {
  themeColors.value = palette.themeColors;
  bgColor.value = palette.bgColor;
  changeColor();
}

function removeThemeColor() {
  if (themeColors.value.length === 1) {
    return;
  }
  themeColors.value.splice(themeColors.value.length - 1, 1);
  emit('change');
}

function addThemeColor() {
  themeColors.value.push(
    themeColors.value.length === 0
      ? '#555'
      : themeColors.value[themeColors.value.length - 1]
  );
  emit('change');
}

function getConfig() {
  return {
    bgColor: bgColor.value,
    themeColors: themeColors.value,
    saturation: saturation.value,
    lightness: lightness.value,
    alpha: alpha.value,
    fontFamily: selectedFontFamily.value,
    fontSize: fontSize.value,
    rotate: rotate.value,
    width: width.value,
    height: height.value,
    shape: selectedMask.value === 'image' ? null : selectedMask.value
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

.color-picker-btn,
.color-palette {
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

.color-palette {
  display: inline-block;
  margin-bottom: 5px;
  width: auto;
  height: 33px;
  padding: 5px;
  cursor: pointer;

  .color {
    display: inline-block;
    width: 20px;
    height: 20px;
    margin-right: 5px;
    border-radius: 3px;

    &:last-child {
      margin-right: 0;
    }
  }
}

.el-checkbox {
  --el-checkbox-font-color: #888;
  margin-top: 7px;
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
