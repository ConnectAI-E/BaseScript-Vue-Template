<script>
  import { bitable } from '@base-open/web-api';
  import { ref, onMounted } from 'vue';
  import {
    ElButton,
    ElForm,
    ElFormItem,
    ElSelect,
    ElOption,
  } from 'element-plus';

  export default {
    components: {
      ElButton,
      ElForm,
      ElFormItem,
      ElSelect,
      ElOption,
    },
    setup() {
      const formData = ref({ table: '' });
      const tableMetaList = ref([]);

      const addRecord = async () => {
        const tableId = formData.value.table;
        if (tableId) {
          const table = await bitable.base.getTableById(tableId);
          table.addRecord({ fields: {} });
        }
      };

      onMounted(async () => {
        const [tableList, selection] = await Promise.all([bitable.base.getTableMetaList(), bitable.base.getSelection()]);
        formData.value.table = selection.tableId;
        tableMetaList.value = tableList;
      });

      return {
        formData,
        tableMetaList,
        addRecord,
      };
    },
  };
</script>

<template>
  <el-form ref="form" class="form" :model="formData" label-position="top">
    <el-form-item label="开发指南">
      <a
        href="https://bytedance.feishu.cn/docx/HazFdSHH9ofRGKx8424cwzLlnZc"
        target="_blank"
        rel="noopener noreferrer"
      >
        扩展脚本开发指南
      </a>
      、
      <a
        href="https://bytedance.feishu.cn/docx/VxhudDXbyo1V7jxAcTbctJQ5nvc"
        target="_blank"
        rel="noopener noreferrer"
      >
        Base Extension Scripts Guide
      </a>
    </el-form-item>
    <el-form-item label="API">
      <a
        href="https://bytedance.feishu.cn/docx/HjCEd1sPzoVnxIxF3LrcKnepnUf"
        target="_blank"
        rel="noopener noreferrer"
      >
        扩展脚本API
      </a>
      、
      <a
        href="https://bytedance.feishu.cn/docx/OPatd1tBZoWogFxKKtmcBZMxnle"
        target="_blank"
        rel="noopener noreferrer"
      >
        Base Extension Scripts Front-end API
      </a>
    </el-form-item>
    <el-form-item label="选择数据表" size="large">
        <el-select v-model="formData.table" placeholder="请选择数据表" style="width: 100%">
          <el-option
            v-for="meta in tableMetaList"
            :key="meta.id"
            :label="meta.name"
            :value="meta.id"
          />
        </el-select>
    </el-form-item>
    <el-button type="primary" plain size="large" @click="addRecord">新增一行记录</el-button>
  </el-form>
</template>

<style scoped>
  .form :deep(.el-form-item__label) {
    font-size: 16px;
    color: var(--el-text-color-primary);
    margin-bottom: 0;
  }
  .form :deep(.el-form-item__content) {
    font-size: 16px;
  }
</style>
