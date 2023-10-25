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
      <el-form :model="form" label-width="160px" >

        <el-form-item label="导入方式">
          <el-radio-group v-model="form.dataType" size="large">
            <el-radio-button label="文本框"  :checked="true"  />
            <el-radio-button label="Excel"/>
          </el-radio-group>
        </el-form-item>

        <el-form-item label="备注">
          <el-input v-model="form.remark" autocomplete="off"  class="el-input"/>
        </el-form-item>

        <!-- 不要将表单隐藏, 否则无法填写上传URL -->
        <el-form-item label="文件">
          <el-upload ref="upload" 
              :action="transferGoodsUrl" 
              :auto-upload="false" 
              :data="form" 
              :on-success="onSuccess"  
            >
            <el-button :type="form.dataType === '文本框' ? 'info' : 'primary'">选择文件</el-button>
          </el-upload>
        </el-form-item>

        <el-form-item label="文本框导入" v-show="form.dataType == '文本框'">
          <el-input v-model="form.text" type="textarea" class="el-input" placeholder="一行一个" />
        </el-form-item>

        <el-form-item label="操作">
          <el-button type="warning" @click="submitUpload">开始导入</el-button>
        </el-form-item>
      </el-form>

  </div>
</template>
  
<script setup lang="ts">
import { inject, reactive, ref, onMounted} from 'vue';
import { ElNotification, UploadInstance } from 'element-plus';
import { transferGoods, transferGoodsUrl } from '/@/api/controllerUrls'


const isOpen = inject('isOpen')
const upload = ref<UploadInstance>();

const form = reactive({
  remark: '',
  uid: "",
  dataType:"文本框",
  text:""
});

const submitUpload = () => {
  // upload.value!.submit();
  console.log("类型", form.dataType)

  if (form.dataType !== '文本框') {
    upload.value!.submit(); // 只有在非文本框模式下提交文件
  } else {
    transferGoods(form).then((res)=> {
      if(res.code === 1){
        ElNotification({message: '导入成功'});
      }
      else {
        ElNotification({message: res.msg});
      }
    })
  }
};

// 上传成功的回调
const onSuccess = (res: any) => {
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

onMounted(() => {
  if (Array.isArray(ids)) {
    form.text = ids.join('\n'); // 将数组元素合并为一行一个元素
  }
});

</script>
<style scoped>
.el-input {
  width: 600px;
}
.file-input[disabled] {
  background-color: #ccc; /* 设置禁用状态的背景颜色 */
  /* 还可以添加其他样式属性 */
}
</style>
```


## 4.后端控制器

```php
public function transferGoods()
{
    $text = $this->request->post('text');
    dd($text);
}
```