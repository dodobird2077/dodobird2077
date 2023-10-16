# 组件间传值, 弹窗传值

## 1.双击ID后, 在弹窗中获取ID的值


1. 主页中, 给ID添加双击事件

```html
<Table ref="tableRef">
    <!-- 插槽 -->
    <template #detail>
        <el-table-column label="详情" width="130">
            <template #default="scope">
                <el-link type="success" @dblclick="openDetail(scope.row.sn)">{{scope.row.sn}}</el-link>
            </template>
        </el-table-column>
    </template>
</Table>
<Detail :id="openId" />
```

2. 主页script中

```html
<script setup lang="ts">
// 1.双击弹窗, 控制弹窗的isOpen使用provide传值,允许子组件中轻松访问父组件的数据，而无需props。
const isOpen = ref(false)

// 2.用于设置子组件标题, 优点是明确的数据来源, 单向数据流, 因为这个值还需要在子组件进行各种使用
const openId = ref('') 

const openDetail = (id: string) => {
    isOpen.value = true
    openId.value = id
}
// 不要直接在openDetail中写, 因为provide方法只能存在setup中
provide('isOpen', isOpen) 
</script>
```

3. 弹窗子组件detail.vue中

```html
<template>
    <div>
         <el-dialog v-model="isOpen" :title="id">
             <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
                 <el-tab-pane label="订单日志" name="content2">
                    <el-descriptions title="订单日志"  :column="2" :border="true">
                      <el-descriptions-item v-for="item in content2" :label="item.title" >{{ item.id }}</el-descriptions-item>
                    </el-descriptions>
                 </el-tab-pane>
             </el-tabs>
         </el-dialog>
    </div>
</template>
<script setup lang='ts'>
  import { ref, inject, defineProps } from 'vue'
  import type { TabsPaneContext } from 'element-plus'
  import {getGoodsInfo, getGoodsOrder} from '/@/api/public/goods/goods'

  const activeName = ref('first')
  const content2 = ref([] as any[]);
  const isOpen = inject('isOpen')  // 弹窗

  // 注意props要放在handleClick前面, 不然props.id没有值
  const props = defineProps({
    id: String, 
  })
  
  const handleClick = (tab: TabsPaneContext, event: Event) => {
      console.log(tab.index);
      switch (tab.index) {
        case '1':
          getGoodsOrder(props.id).then( (res) => {
            content2.value = res.data.list
          })
          break;
      }
  }
</script>
```

4. 其他(可忽略)

```ts
// goods.ts中
export function getGoodsOrder(id: any) {
    return createAxios({
        url: '/admin/goods/getGoodsOrder?id='+id,
        method: 'get',
    })
}
```