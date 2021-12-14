<template>
    <div>
        <el-table class="word-table"
            :data="tableData"
        >
            <el-table-column
                fixed
                prop="name"
                label="文本"
            >
                <template v-slot="scope">
                    <el-input
                        v-if="scope && scope.row.editAttr === 'name'"
                        size="small"
                        v-model="scope.row.name"
                        @blur="doneEditing(scope.row)"
                    >
                    </el-input>
                    <span
                        v-if="scope && scope.row.editAttr !== 'name'"
                        @click="edit(scope.row, 'name')"
                    >
                        {{ scope.row.name }}
                    </span>
                </template>
            </el-table-column>
            <el-table-column
                prop="value"
                label="字号"
            >
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
            <el-table-column
                label="操作"
                width="50"
            >
                <template v-slot="scope">
                    <el-button
                        type="text"
                        size="small"
                        @click="removeData(scope.row)"
                    >
                        删除
                    </el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-table class="word-table"
            :show-header="false"
            :data="pendingData"
        >
            <el-table-column
                fixed
                prop="name"
                label="文本"
            >
                <template v-slot="scope">
                    <el-input
                        size="small"
                        v-model="scope.row.name"
                    >
                    </el-input>
                </template>
            </el-table-column>
            <el-table-column
                prop="value"
                label="字号"
            >
                <template v-slot="scope">
                    <el-input
                        size="small"
                        v-model="scope.row.value"
                    >
                    </el-input>
                </template>
            </el-table-column>
            <el-table-column
                label="操作"
                width="50"
            >
                <template v-slot="scope">
                    <el-button
                        type="text"
                        size="small"
                        @click="addRow()"
                    >
                        添加
                    </el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

type WordData = {
    name?: string;
    value?: number;
    editAttr?: string;
};

const tableData = ref([{name: '测试', value: 100}] as WordData[]);
const pendingData = ref([{}] as WordData[]);

function removeData(row: WordData) {
    tableData.value = tableData.value.filter(item => item !== row);
}

function edit(row: WordData, attr: 'name' | 'value') {
    console.log('edit', row, attr)
    row.editAttr = attr;
}

function doneEditing(row: WordData) {
    row.editAttr = undefined;
}

function addRow() {
    if (pendingData.value[0].name && pendingData.value[0].value) {
        const value = parseFloat(pendingData.value[0].value as unknown as string);
        if (isNaN(value)) {
            alert('请输入数字');
            return;
        }
        else {
        tableData.value.push({
            name: pendingData.value[0].name,
            value
        });
        pendingData.value = [{}];
        }
    }
}
</script>

<style lang="scss">
.word-table {
    width: 100%;
}
</style>
