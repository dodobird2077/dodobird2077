# 自定义添加页面, 带文件导入及文本框二选一


1. 首先禁用列表页的添加, b并增加自定义按钮
```html
<template>
    <div class="default-main ba-table-box">
        <el-alert class="ba-table-alert" v-if="baTable.table.remark" :title="baTable.table.remark" type="info" show-icon />

        <TableHeader
            :buttons="['refresh', 'comSearch', 'quickSearch', 'columnDisplay']"
            :quick-search-placeholder="t('Quick search placeholder', { fields: t('transferLog.quick Search Fields') })"
        >
            <el-button @click="isOpen = true">发起转让</el-button>  
        </TableHeader>

        <Table ref="tableRef"></Table>

        <PopupForm />

        <!-- 弹窗转让表单 -->
        <OpenTransfer />
    </div>
</template>

<script setup lang="ts">
// 1.开启转让弹窗 (上面省略部分)
const isOpen = ref(false)
provide('isOpen', isOpen)
</script>
```

2. `重点:` 弹窗转让表单

```html
<template>
  <div class="centered-container">
    <el-dialog v-model="isOpen" title="发起转让">
      <el-form :model="form" label-width="160px" >

        <el-form-item label="导入方式">
          <el-radio-group v-model="form.dataType" size="large">
            <el-radio-button label="Excel" :checked="true" />
            <el-radio-button label="文本框" />
          </el-radio-group>
        </el-form-item>
      
        <el-form-item label="用户">
            <el-select v-model="form.uid" placeholder="用户">
              <el-option v-for="item in userList" :key="item.id" :label="item.title" :value="item.id" />
            </el-select>
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

  </el-dialog>
  </div>
</template>
  
<script setup lang="ts">
import { inject, reactive, ref} from 'vue';
import { ElNotification, UploadInstance } from 'element-plus';
import { getUserList } from '/@/api/public/member/member'
import { transferGoods, transferGoodsUrl } from '/@/api/controllerUrls'

const userList = ref([] as any[]);
getUserList().then((res)=> {
    userList.value = res.data.list
})

const isOpen = inject('isOpen')
const upload = ref<UploadInstance>();

const form = reactive({
  remark: '',
  uid: "",
  dataType:"Excel",
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

3. 封装带文件上传和不带文件上传的API请求 `web\src\api\controllerUrls.ts`

```js
// 商品转让: 1.文件导入, 2.text导入
export const transferGoodsUrl = url + '/index.php/admin/goods/tramsferGoods?batoken=' + token 
export function transferGoods(form: any) {
    return createAxios({
        url: '/admin/goods/tramsferGoods',
        method: 'post',
        data: form
    })
}
```

4. 后端控制器 `app\admin\controller\Goods.php`

```php
class Goods extends Backend
{
   // 转让商品
    public function tramsferGoods()
    {
        $dataType = $this->request->post('dataType');
        $uid =  $this->request->post('uid');
        $remark =  $this->request->post('remark');
        $memberModel = new Member();

        if ($dataType == 'Excel') {
            $data = $this->getExcelData(true); // 获取Excel表格数据
        } else {
            $data = $this->getTextData(); // 获取文本框导入数据
        }

        Db::startTrans();
        try {
            // 执行数据库操作
            foreach ($data as $item) {
                $seller_id = $memberModel->getSellerId(trim($item[2]));
                if (! $seller_id) {
                    throw new \Exception("销售不存在" . $item[2]);
                }
                $insert = [
                    'code'    => trim($item[0]),
                    'title' => trim($item[1]),
                    'seller_id' => $seller_id,
                    'weight'    => trim($item[3])
                ];
                $this->model->create($insert);
            }

            // 如果所有操作都成功，提交事务
            Db::commit();
        } catch (\Exception $e) {
            // 如果出现异常，回滚事务
            Db::rollback();
            $this->error($e->getMessage());
        }
        $this->success("导入成功");
    }

}
```

5. 在控制器的公共父类中, 定义获取文件上传数据的方法

```php
class Backend extends Api
{

    /***************************************自定义封装方法 *********************************/
    /**
     * 获取表格数据
     */
    public function getExcelData($isOneColumn = true)
    {
        $file = $this->request->file('file');
        if (! $file) {
            return [];
        }
        $filePath = $file->getPathname();
        $spreadsheet = IOFactory::load($filePath);
        $sheet       = $spreadsheet->getSheet(0);
        $data        = $sheet->toArray();
        if (empty($data)) {
            return [];
        }
        array_shift($data); // 去掉表头

        // 如果只有一列
        if ($isOneColumn) {
            $data = array_column($data, '0');
        }

        return $data;
    }


    // 获取文本框导入数据
    public function getTextData()
    {
        $text = $this->request->post('text');
        dd($text);
    }
}
```