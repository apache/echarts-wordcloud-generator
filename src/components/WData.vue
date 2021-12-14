<template>
    <div>
        <div class="table" ref="tableRef">
        </div>
    </div>
</template>


<script setup lang="ts">
import { toRefs, PropType } from 'vue';

const props = defineProps({
  data: Array as PropType<{ value: number, name: string }[]>
});
const { data } = toRefs(props);
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
