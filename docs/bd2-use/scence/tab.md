# 4.双击弹窗Tab页,API获取数据


### 1.列表页面
```html
<Table ref="tableRef">
    <!-- 插槽 -->
    <template #detail>
        <el-table-column label="详情" width="130">
            <template #default="scope">
                <el-link type="success" @dblclick="isOpen = true">{{scope.row.sn}}</el-link>
            </template>
        </el-table-column>
    </template>
</Table>
<!-- 详情tab弹窗 -->
<Detail />


<script setup lang="ts">
import Detail from './detail.vue'

{ label: t('goods.sn'), prop: 'id', render: 'slot', slotName: 'detail', operator: 'LIKE' },
</script>
```

### 2.详情弹窗
`web\src\views\backend\goods\detail.vue`

```html
<template>
  <div>
       <el-dialog v-model="isOpen" :title="'商品ID: '+id">

           <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
               <el-tab-pane label="商品信息" name="content1">
                  <el-descriptions title="商品信息"  :column="2" :border="true">
                      <el-descriptions-item v-for="item in content1" :label="item.title" >{{ item.id }}</el-descriptions-item>
                    </el-descriptions>
               </el-tab-pane>

               <el-tab-pane label="订单日志" name="content2">
                  <el-descriptions title="订单日志"  :column="2" :border="true">
                    <el-descriptions-item v-for="item in content2" :label="item.title" >{{ item.id }}</el-descriptions-item>
                  </el-descriptions>
               </el-tab-pane>

               <el-tab-pane label="采购日志" name="content3">{{  content3 }}</el-tab-pane>

               <el-tab-pane label="库存日志" name="content4">{{  content4 }}</el-tab-pane>
           </el-tabs>
       </el-dialog>
  </div>
</template>

<script setup lang='ts'>
import { ref, inject, onMounted, watch } from 'vue'
import type { TabsPaneContext } from 'element-plus'
import {getGoodsInfo, getGoodsOrder} from '/@/api/public/goods/goods'

// 设为1的话, 无法加载首次数据
const activeName = ref('content1')

const content1 = ref([] as any[]);
const content2 = ref([] as any[]);
const content3 = ref();
const content4 = ref();

const props = defineProps({
  id: String, // 假设 title 是字符串类型，根据你的实际需要修改
})


const handleClick = (tab: TabsPaneContext, event: Event) => {
    console.log(tab.index);
    switch (tab.index) {
      case '0':
        getGoodsInfo(props.id).then( (res) => {
          content1.value = res.data.list
        })
        break;
      case '1':
        // content2.value = [{title:"张三", id: 90}, {title:"李四", id: 23}];
        getGoodsOrder(props.id).then( (res) => {
          content2.value = res.data.list
        })
        break;
      case '2':
        content3.value = "获取采购日志";
        break;
      case '3':
        content4.value = "获取库存日志";
        break;
    }
}
// 弹窗
const isOpen = inject('isOpen')

watch(props, (newProps) => {
  console.log('id changed:', newProps.id);
  handleClick({ index: '1' } as TabsPaneContext, new Event('click'));
});

</script>
<style>
.demo-tabs > .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}
</style>
```

### 3.封装的请求
`web\src\api\public\goods\goods.ts`

```ts
import createAxios from '/@/utils/axios'

export function getGoodsInfo(id: any) {
    return createAxios({
        url: '/admin/goods/getGoodsInfo?id='+id,
        method: 'get',
    })
}

export function getGoodsOrder(id: any) {
    return createAxios({
        url: '/admin/goods/getGoodsOrder?id='+id,
        method: 'get',
    })
}
```


### 4.控制器
```php
// 获取商品订单列表
public function getGoodsOrder()
{
    $list = [
        ['id'=> 1, 'title'=> '张三'],
        ['id'=> 2, 'title'=> '李四'],
        ['id'=> 3, 'title'=> '王五'],
    ];
    $this->success('', [
        'list'   => $list,
        'total'  => 10,
    ]);
}
```