# 如何封装API请求和url

## 1.封装url
1. 如: 封装导入导出的url, 远程下拉的URL

```ts
import { useAdminInfo } from "../stores/adminInfo"
import { getUrl } from '/@/utils/axios'
const adminInfo = useAdminInfo()
const url = getUrl()
const token = adminInfo.getToken()

// 用户组远程下拉
export const groupSelect = '/admin/member/groupSelect/'

// 导入和导入的URL
export const importGoods = url + '/index.php/admin/goods/importGoods?batoken=' + token 
export const exportGoods = url + '/admin/goods/exportGoods?server=1'+ '&batoken=' + token 
```

2. 使用
- 2.1 远程下拉使用封装的url: groupSelectUrl

```html
<FormItem label="角色" 
    type="remoteSelect" 
    v-model="baTable.form.items!.group_id" 
    prop="group_id" 
    :input-attr="{ pk: 'id', field: 'title', 'remote-url': groupSelectUrl }"
/>

<script setup lang="ts">
import { groupSelectUrl } from '/@/api/controllerUrls' 
</script>
```
- 2.2 导出中使用url

```html
<script setup lang="ts">
const exportExcel = () => {
    window.location.href = exportGoods // 在需要的地方, @click="exportExcel"
}
</script>
```

## 2.封装API请求, 获取列表数据

1. 在`src\api\public\member\member.ts`中封装api请求
```ts
// 获取用户列表,一位数组
export function getUserList() {
    return createAxios({
        url: '/admin/member/getUserList',
        method: 'get',
    })
}
```

2. 在需要的地方使用
```ts
import {getUserList} from '/@/api/public/member/member'
const mealList = ref([] as any[]);
getMealList().then((res)=> {
    mealList.value = res.data.list
})
```

