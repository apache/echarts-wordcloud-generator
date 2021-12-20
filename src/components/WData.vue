<template>
  <div>
    <el-table class="word-table" :data="tableData" empty-text="无数据">
      <el-table-column prop="name" label="文本">
        <template v-slot="scope">
          <el-input
            v-if="!scope || scope.row.editAttr === 'name'"
            size="small"
            v-model="scope.row.name"
            @blur="doneEditing(scope.row)"
          >
          </el-input>
          <span
            v-if="!scope || scope.row.editAttr !== 'name'"
            @click="edit(scope.row, 'name')"
          >
            {{ scope.row.name }}
          </span>
        </template>
      </el-table-column>
      <el-table-column prop="value" label="相对大小">
        <template v-slot="scope">
          <el-input
            v-if="scope && scope.row.editAttr === 'value'"
            size="small"
            v-model="scope.row.value"
            @blur="doneEditing(scope.row)"
          >
          </el-input>
          <span
            v-if="scope && scope.row.editAttr !== 'value'"
            @click="edit(scope.row, 'value')"
          >
            {{ scope.row.value }}
          </span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="50">
        <template v-slot="scope">
          <el-button type="text" size="small" @click="removeData(scope.row)">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-table class="word-table" :show-header="false" :data="pendingData">
      <el-table-column fixed prop="name" label="文本">
        <template v-slot="scope">
          <el-input size="small" v-model="scope.row.name"> </el-input>
        </template>
      </el-table-column>
      <el-table-column prop="value" label="大小">
        <template v-slot="scope">
          <el-input
            size="small"
            v-model="scope.row.value"
            placeholder="留空则随机大小"
          >
          </el-input>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="50">
        <template v-slot="scope">
          <el-button type="text" size="small" @click="addRow()">
            添加
          </el-button>
        </template>
      </el-table-column>
    </el-table>
    <div class="padding">
      <el-button type="primary" size="medium" @click="removeAll()">
        导入 .cvs
      </el-button>
      <el-button type="danger" size="medium" @click="removeAll()">
        清空
      </el-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineEmits } from "vue";

type WordData = {
  name?: string;
  value?: number;
  editAttr?: string;
};

const tableData = ref([] as WordData[]);
const pendingData = ref([{}] as WordData[]);

const emit = defineEmits(["change"]);

defineExpose({ data: tableData, setData });

function setData(data: WordData[]) {
  tableData.value = data;
  emit("change");
}

function removeData(row: WordData) {
  tableData.value = tableData.value.filter(item => item !== row);
  emit("change");
}

function edit(row: WordData, attr: "name" | "value") {
  row.editAttr = attr;
}

function doneEditing(row: WordData) {
  row.editAttr = undefined;
  emit("change");
}

function addRow() {
  if (pendingData.value[0].name) {
    const rawValue = (pendingData.value[0].value as unknown) as string;
    const value = parseFloat(rawValue);
    if (rawValue != null && rawValue !== "" && isNaN(value)) {
      alert("请输入数字");
      return;
    } else {
      tableData.value.push({
        name: pendingData.value[0].name,
        value
      });
      pendingData.value = [{}];
      emit("change");
    }
  }
}

function removeAll() {
  tableData.value = [];
  emit("change");
}
</script>

<style lang="scss">
.word-table {
  width: 100%;
  max-height: 400px;
  overflow: auto;
}

.padding {
  padding: 10px;
}
</style>
