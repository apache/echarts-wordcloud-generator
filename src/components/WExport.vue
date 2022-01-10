<template>
  <h5>画布尺寸</h5>
  <el-row>
    <el-col :span="4">
      <h5>宽度</h5>
    </el-col>
    <el-col :span="7">
      <el-input
        type="number"
        v-model="width"
        :min="100"
        :max="2000"
        :step="10"
        size="small"
        @change="onSizeChange"
      >
      </el-input>
    </el-col>
    <el-col :span="4" :offset="1">
      <h5>高度</h5>
    </el-col>
    <el-col :span="7">
      <el-input
        type="number"
        v-model="height"
        :min="100"
        :max="2000"
        :step="10"
        size="small"
        @change="onSizeChange"
      >
      </el-input>
    </el-col>
  </el-row>
  <div class="padding">
    <el-button type="primary" size="medium" @click="download()">
      下载图片
    </el-button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const width = ref('800');
const height = ref('600');

defineExpose({ getSize });

const emit = defineEmits(['sizeChange', 'download']);

function getSize() {
  return {
    width: parseInt(width.value, 10),
    height: parseInt(height.value, 10)
  };
}

function onSizeChange() {
  emit('sizeChange', getSize());
}

function download() {
  emit('download');
}
</script>

<style lang="scss">
.padding {
  padding: 20px 0;
}
</style>
