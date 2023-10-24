# 批量勾选操作

## 1.添加操作页面菜单
![logo](/../../_media/img/bd-renew.png ':size=700')

`/src/views/backend/goods/renewForm.vue`

## 2.首页添加自定义操作按钮

```html
<TableHeader
    :buttons="['refresh', 'add', 'edit', 'delete', 'comSearch', 'quickSearch', 'columnDisplay']"
    :quick-search-placeholder="t('Quick search placeholder', { fields: t('goods.quick Search Fields') })"
>
    <el-button style="margin-left:10px" class="table-header-operate" type="success" @click="exportExcel">
        <span class="table-header-operate-text">导出</span>
    </el-button>

    <!-- 示例核心代码(1/1) -->
    <el-button @click="onSelectAll" v-blur class="table-header-operate" type="success">
        <span class="table-header-operate-text">延长</span>
    </el-button>

</TableHeader>

<script setup lang="ts">
import { useRouter } from 'vue-router'


const router = useRouter()
const onSelectAll = () => {
    // let ref = tableRef.value.getRef()
    let ids = baTable.getSelectionIds()
    console.log('选中的行: ', ids);
    router.push({ path: '/admin/goods/renew', query: { ids: ids } })
}
</script>
```

## 3.操作页面代码

```html
<template>
    <div class="centered-container">
      <el-form :model="form">
        <el-form-item label="备注">
          <el-input v-model="form.remark" autocomplete="off" />
        </el-form-item>
        <el-form-item label="文件">
          <el-upload ref="upload" :action="importGoods" :auto-upload="false" :data="form" :on-success="onSuccess">
            <el-button type="primary">选择文件</el-button>
          </el-upload>
        </el-form-item>
      </el-form>
      <span class="dialog-footer">
        <el-button type="warning" @click="submitUpload">开始导入</el-button>
        <el-button>清空</el-button>
      </span>
    </div>
  </template>
  
  <script setup lang="ts">
  import { reactive, ref} from 'vue';
  import { ElNotification, UploadInstance } from 'element-plus';
  import {importGoods} from '/@/api/controllerUrls'
  
  const upload = ref<UploadInstance>();
  
  const submitUpload = () => {
    upload.value!.submit();
  };
  const form = reactive({
    remark: ''
  });

  // 上传成功的回调
  const onSuccess = (res: any) => {
    console.log(res);
    if (res.code === 1) {
      ElNotification({message: '导入成功'});
    } else {
      ElNotification({message: res.msg});
    }
  };

  import { useRouter } from 'vue-router';

  const router = useRouter();
  const ids = router.currentRoute.value.query.ids;

  console.log('@@@ids', ids); // 获取到路由参数的值
  </script>
  
  <style scoped>
  /* 将元素居中 */
  .centered-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 50%; /* 宽度占用50% */
    margin: 100px auto; /* 水平居中 */
  }
  </style>
```