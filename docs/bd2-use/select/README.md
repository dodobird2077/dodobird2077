# 下拉列表使用



## A.远程下拉列表

### A1.列表页中 `index.vue`
```ts
// 导入user管理的控制器Url-- 1.0版本的文件
import { goodsCategory } from '/@/api/controllerUrls' 

{ 
    label: '分类', 
    prop: 'category_id', 
    comSearchRender: 'remoteSelect', 
    show:false, 
    remote: {
        pk: 'id',
        field: 'title',
        remoteUrl: goodsCategory +'index'            
    }
},
```
`web\src\api\controllerUrls.ts`
```ts
// 这里其实就是商品分类的菜单, 可灵活变更为其他数据
export const goodsCategory = '/admin/goodsCategory/'
export const groupSelect = '/admin/member/groupSelect/'
```

### A2.编辑页中 `这里以'添加用户'页面为例`
1. 首页的添加用户按钮, 这里忽略
2. 编辑页面中
```html
<FormItem 
    label="角色" 
    type="remoteSelect" 
    v-model="baTable.form.items!.group_id" 
    prop="group_id" 
    :input-attr="{ 
        pk: 'id', 
        field: 'title', 
        'remote-url': groupSelect 
    }"
/>

<script setup lang="ts">
import { groupSelect } from '/@/api/controllerUrls' 
</script>
```

`web\src\api\controllerUrls.ts`
```ts
// 活用用户组数据
export const groupSelect = '/admin/member/groupSelect/'
```


## B.普通下拉列表

### B.1 自定义页面中
```html
<el-form :model="form">

        <el-form-item label="用户">
            <el-select v-model="form.uid" placeholder="用户">
              <el-option v-for="user in userList" :key="user.id" :label="user.title" :value="user.id" />
            </el-select>
        </el-form-item>

        <el-form-item label="">
            <el-select v-model="form.mobile_id" placeholder="">
                <el-option v-for="(value,key) in mobileConfig"  :key="key" :label="value" :value="key" />
            </el-select>
        </el-form-item>
</el-form>

<script setup lang="ts">
import mobileConfig from '/@/api/config/sim/mobile'
import {getUserList} from '/@/api/public/member/member'
</script>
```
- 封装的获取下拉列表API请求
`\web\src\api\backend\public\`下
```ts
import createAxios from '/@/utils/axios'
// 获取用户列表,一位数组
export function getUserList() {
    return createAxios({
        url: '/admin/member/getUserList',
        method: 'get',
    })
}
```

```ts
// mobileConfig
export default {
    1: '单人套餐',
    2: '双人火锅套餐',
    3: '多人火锅套餐'
}
```
