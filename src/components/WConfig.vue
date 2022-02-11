<template>
  <el-collapse>
    <el-collapse-item title="颜色" name="color">
      <h5>配色方案</h5>
      <div>
        <div
          class="color-palette"
          v-for="palette in colorPalettes"
          v-bind:key="palette.bgColor"
          :style="{ background: palette.bgColor }"
          @click="useColorPalette(palette)"
        >
          <div
            class="color"
            v-for="color in palette.themeColors"
            v-bind:key="color"
            :style="{ background: color }"
          ></div>
        </div>
      </div>

      <h5>背景色</h5>
      <el-color-picker v-model="bgColor" size="small" @change="change">
      </el-color-picker>

      <h5>色相范围</h5>
      <el-row>
        <el-col :span="22">
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
        <!-- <el-col :span="8" :offset="2">
          <el-checkbox>关联数据大小</el-checkbox>
        </el-col> -->
      </el-row>

      <h5>饱和度范围</h5>
      <el-row>
        <el-col :span="22" :offset="1">
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
        <!-- <el-col :span="8" :offset="2">
          <el-checkbox>关联数据大小</el-checkbox>
        </el-col> -->
      </el-row>

      <h5>亮度范围</h5>
      <el-row>
        <el-col :span="22" :offset="1">
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
        <!-- <el-col :span="8" :offset="2">
          <el-checkbox>关联数据大小</el-checkbox>
        </el-col> -->
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
            <el-option-group
              v-for="group in fontFamilies"
              :key="group.name"
              :label="group.name"
            >
              <el-option
                v-for="item in group.children"
                :key="item"
                :label="item.name"
                :value="item.value"
              >
              </el-option>
            </el-option-group>
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
        <el-col :span="12" class="header-right">
          <el-checkbox v-model="shapeRatio" @change="change">
            保持长宽比
          </el-checkbox>
        </el-col>
      </el-row>
      <div
        class="img-select"
        v-for="item in masks"
        :key="item.value"
        :value="item.value"
        v-bind:class="{ selected: item.value === selectedMask }"
        v-bind:title="item.name"
        @click="changeMask(item.value)"
      >
        <img v-bind:src="item.value + '.png'" />
        <div class="mark" v-if="item.isFromFreepik">*</div>
      </div>
      <el-upload
        class="img-select img-uploader"
        v-bind:class="{ selected: imageUrl === selectedMask }"
        action="https://jsonplaceholder.typicode.com/posts/"
        :show-file-list="false"
        list-type="picture"
        :on-success="handleImageSuccess"
        :before-upload="beforeImageUpload"
      >
        <img v-if="imageUrl" :src="imageUrl" />
        <i v-else class="el-icon-plus img-uploader-icon"></i>
      </el-upload>
      <div class="hint">
        带 * 的形状来自
        <a href="https://www.freepik.com" title="Freepik" _target="_blank"
          >Freepik</a
        >，查看<a
          href="https://media.flaticon.com/license/license.pdf"
          _target="_blank"
          >版权</a
        >
      </div>
    </el-collapse-item>
  </el-collapse>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import WebFont from 'webfontloader';

const colorPalettes = [
  {
    bgColor: '#f8eddc',
    themeColors: ['#f19d70', '#b7b7a4', '#6b705c']
  },
  {
    bgColor: '#ffeff1',
    themeColors: ['#4C3F91', '#9145B6', '#B958A5', '#FF5677']
  },
  {
    bgColor: '#eef6fa',
    themeColors: ['#5470c6', '#91cc75', '#fac858', '#ee6666']
  },
  {
    bgColor: '#FFE6E2',
    themeColors: ['#23425F', '#A64942', '#FF7844']
  },
  {
    bgColor: '#FFF8E5',
    themeColors: ['#EA5C2B', '#FF7F3F', '#F6D860', '#95CD41']
  },
  {
    bgColor: '#F0F0F0',
    themeColors: ['#504D4D', '#FF5677', '#F5B316']
  }
];

const bgColor = ref(colorPalettes[0].bgColor);
const themeColors = ref(colorPalettes[0].themeColors);
const saturation = ref([20, 60]);
const lightness = ref([50, 75]);
const alpha = ref([0.5, 0.8]);
const selectedFontFamily = ref('Arial');
const fontSize = ref([4, 100]);
const rotate = ref([-90, 90]);
const width = ref(90);
const height = ref(90);
const selectedMask = ref('heart');
const shapeRatio = ref(true);
const imageUrl = ref('');

const normalizeFont = (font: string | { name: string; value: string }) => {
  if (typeof font === 'string') {
    return {
      name: font,
      value: font
    };
  }
  return font;
};

const sortFont = (
  a: { name: string; value: string },
  b: { name: string; value: string }
) => {
  if (a.name < b.name) {
    return -1;
  }
  if (a.name > b.name) {
    return 1;
  }
  return 0;
};

const fontFamilies = [
  {
    name: '系统字体',
    children: [
      'Arial',
      'Times New Roman',
      'Courier New',
      'Georgia',
      { name: '宋体/华文宋体', value: 'SimSun, STSong' },
      { name: '黑体/华文黑体', value: 'SimHei, STHeiti' },
      { name: '微软雅黑', value: 'Microsoft YaHei' }
    ]
      .map(normalizeFont)
      .sort(sortFont)
  },
  {
    name: 'Google Fonts',
    children: [
      'Pushster',
      'Lato',
      'Roboto',
      'Roboto Slab',
      'Open Sans',
      'Oswald',
      'Ubuntu',
      'Lobster',
      { name: '马善政毛笔楷书', value: 'Ma Shan Zheng' },
      { name: '站酷庆科黄油体', value: 'ZCOOL QingKe HuangYou' },
      { name: '站酷小薇LOGO体', value: 'ZCOOL XiaoWei' }
    ]
      .map(normalizeFont)
      .sort(sortFont)
  }
];
const webFontsLoaded: string[] = [];

const masks = [
  {
    name: 'ECharts 0',
    value: 'echarts-0'
  },
  {
    name: 'ECharts 1',
    value: 'echarts-1'
  },
  {
    name: '圆形',
    value: 'circle'
  },
  {
    name: '方形',
    value: 'rect'
  },
  {
    name: '云',
    value: 'cloud',
    isFromFreepik: true
  },
  {
    name: '菱形',
    value: 'rhombus',
    isFromFreepik: true
  },
  {
    name: '三角形',
    value: 'triangle',
    isFromFreepik: true
  },
  {
    name: '五边形',
    value: 'pentagon',
    isFromFreepik: true
  },
  {
    name: '五角星',
    value: 'star',
    isFromFreepik: true
  },
  {
    name: '圆角矩形',
    value: 'rounded-rectangle',
    isFromFreepik: true
  },
  {
    name: '爱心',
    value: 'heart',
    isFromFreepik: true
  },
  {
    name: '六边形',
    value: 'hexagonal',
    isFromFreepik: true
  },
  {
    name: '闪电',
    value: 'flash',
    isFromFreepik: true
  },
  {
    name: '水滴',
    value: 'drop',
    isFromFreepik: true
  },
  {
    name: '六瓣花',
    value: 'flower',
    isFromFreepik: true
  },
  {
    name: '喷溅',
    value: 'splash',
    isFromFreepik: true
  },
  {
    name: '地标',
    value: 'maps-and-flags',
    isFromFreepik: true
  },
  {
    name: '问号',
    value: 'question-mark',
    isFromFreepik: true
  },
  {
    name: 'WOW',
    value: 'wow'
  }
];

const emit = defineEmits(['change', 'fontLoading', 'fontLoaded']);

defineExpose({ getConfig });

setTimeout(() => {
  changeColor();
}, 0);

function changeFontFamily() {
  let fontFamily = selectedFontFamily.value;
  const font = fontFamilies[1].children.find(
    (item) => item.value === fontFamily
  );
  if (font && !webFontsLoaded.includes(font.value)) {
    emit('fontLoading');
    WebFont.load({
      google: {
        families: [fontFamily]
      },
      fontactive: () => {
        webFontsLoaded.push(font.value);
        emit('fontLoaded');
        change();
      }
    });
    return;
  }

  change();
}

function changeMask(value: string) {
  selectedMask.value = value;
  change();
}

function changeColor() {
  // let minS = 100;
  // let maxS = 0;
  // let minL = 100;
  // let maxL = 0;
  // themeColors.value.forEach((color) => {
  //   const c = Color(color);
  //   const s = Math.round(c.saturationv());
  //   const l = Math.round(c.lightness());
  //   if (s < minS) {
  //     minS = s;
  //   }
  //   if (s > maxS) {
  //     maxS = s;
  //   }
  //   if (l < minL) {
  //     minL = l;
  //   }
  //   if (l > maxL) {
  //     maxL = l;
  //   }
  // });
  // saturation.value = [minS, maxS];
  // lightness.value = [minL, maxL];
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

function handleImageSuccess(res: any, file: any) {
  // console.log(res, file);
  imageUrl.value = URL.createObjectURL(file.raw);
  selectedMask.value = imageUrl.value;
  change();
}

function beforeImageUpload(file: any) {
  // TODO: loading
  return true;
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
    shape: selectedMask.value,
    shapeRatio: shapeRatio.value
  };
}
</script>

<style lang="scss">
$brand-color: #409eff;
$border-color: #e6e6e6;

.title-right {
  position: absolute;
  top: 10px;
  right: 0;

  a {
    display: inline-block;
  }
}

.header-right {
  text-align: right;
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
  border: 1px solid $border-color;
  border-radius: 4px;
  color: $brand-color;
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

.hint {
  margin: 5px 0;
  color: #888;

  a {
    color: $brand-color;
  }
}

.img-select {
  position: relative;
  display: inline-block;
  width: 57px;
  height: 57px;
  margin-right: 5px;
  border: 1px solid $border-color;
  padding: 5px;
  border-radius: 4px;
  opacity: 0.5;
  cursor: pointer;

  &.selected {
    border-color: $brand-color;
    opacity: 1;
  }

  img {
    max-width: 100%;
    max-height: 100%;
  }

  .mark {
    position: absolute;
    top: -2px;
    right: 4px;
    color: #000;
  }
}

.img-uploader {
  position: relative;
  overflow: hidden;
  top: 6px;
}

.img-uploader-icon {
  font-size: 28px;
  line-height: 55px;
  text-align: center;
}

.el-upload {
  display: block;
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
  color: $brand-color;
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
