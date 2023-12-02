# 封装用户管理

1. 封装用户管理 `待完善`



## 一.注意事项
- 1.用户组的远程下拉,是通过在后台添加一个按钮菜单来实现权限
- 2.添加和编辑分开了页面


## 二.步骤

### 1.先使用快速CURD生成基础代码, 使用系统用户表

### 2.修改控制器代码 `app\admin\controller\Member.php`

```php
<?php
namespace app\admin\controller;

use app\admin\library\Auth;
use app\admin\model\Admin;
use ba\Random;
use Throwable;
use app\common\controller\Backend;
use think\facade\Db;

/**
 * 用户管理管理
 */
class Member extends Backend
{
    /**
     * Member模型对象
     * @var object
     * @phpstan-var \app\admin\model\Member
     */
    protected object $model;

    protected array|string $preExcludeFields = ['id', 'email', 'mobile', 'login_failure', 'last_login_time', 'last_login_ip', 'salt', 'motto', 'update_time', 'create_time', 'user_path'];

    protected array $withJoinTable = ['pidTable'];

    protected string|array $quickSearchField = ['username', 'nickname', 'id'];

    protected $uid = null;

    public function initialize(): void
    {
        parent::initialize();
        $this->model = new \app\admin\model\Member;
        $this->uid = $this->auth->getAdmin()->id;
    }

    /**
     * 查看
     * @throws Throwable
     */
    public function index(): void
    {
        // 如果是 select 则转发到 select 方法，若未重写该方法，其实还是继续执行 index
        if ($this->request->param('select')) {
            $this->select();
        }

        /**
         * 1. withJoin 不可使用 alias 方法设置表别名，别名将自动使用关联模型名称（小写下划线命名规则）
         * 2. 以下的别名设置了主表别名，同时便于拼接查询参数等
         * 3. paginate 数据集可使用链式操作 each(function($item, $key) {}) 遍历处理
         */
        list($where, $alias, $limit, $order) = $this->queryBuilder();
        $res = $this->model
            ->withJoin($this->withJoinTable, $this->withJoinType)
            ->alias($alias)
            ->where($where)
            ->order($order)
            ->paginate($limit);
        $res->visible(['pidTable' => ['username']]);

        $this->success('', [
            'list'   => $res->items(),
            'total'  => $res->total(),
            'remark' => get_route_remark(),
        ]);
    }

    /**
     * 若需重写查看、编辑、删除等方法，请复制 @see \app\admin\library\traits\Backend 中对应的方法至此进行重写
     */

    /**
     * 重写添加和编辑
     */
    public function add(): void
    {
        if ($this->request->isPost()) {
            $data = $this->request->post();
            if (!$data) {
                $this->error(__('Parameter %s can not be empty', ['']));
            }
            $data = $this->excludeFields($data);
            if ($this->dataLimit && $this->dataLimitFieldAutoFill) {
                $data[$this->dataLimitField] = $this->auth->id;
            }


            $result = false;
            $this->model->startTrans();
            try {
                // 模型验证
                if ($this->modelValidate) {
                    $validate = str_replace("\\model\\", "\\validate\\", get_class($this->model));
                    if (class_exists($validate)) {
                        $validate = new $validate;
                        if ($this->modelSceneValidate) $validate->scene('add');
                        $validate->check($data);
                    }
                }

                $salt   = Random::build('alnum', 16);
                $passwd = encrypt_password($data['password'], $salt);

                $insert = [];
                $insert['salt']       = $salt;
                $insert['password']   = $passwd;
                $insert['nickname']   = trim($data['nickname']);
                $insert['username']   = trim($data['username']);
                $insert['createtime'] = time();
                if ($this->auth->isSuperAdmin()) {
                    $insert['pid'] = 0;
                } else {
                    $insert['pid'] = $this->uid;
                }

                $result  = $this->model->save($insert); // 1.添加用户

                $group_arr  = [$data['group_id']]; // 格式化成权限需要的模式

                if ($group_arr) {
                    $groupAccess = [];
                    foreach ($group_arr as $data) {
                        $groupAccess[] = [
                            'uid'      => $this->model->id, // 新建用户ID
                            'group_id' => $data,
                        ];
                    }
                    // [0 =>  ["uid" => 6, "group_id" => "3"] ]
                    Db::name('admin_group_access')->insertAll($groupAccess); // 2.添加用户权限关联
                }
                $this->model->commit();
            } catch (Throwable $e) {
                $this->model->rollback();
                $this->error($e->getMessage());
            }
            if ($result !== false) {
                $this->success(__('Added successfully'));
            } else {
                $this->error(__('No rows were added'));
            }
        }

        $this->error(__('Parameter error'));
    }

    /**
     * 编辑
     * @throws Throwable
     */
    public function edit($id = null): void
    {
        $row = $this->model->find($id);
        if (!$row) {
            $this->error(__('Record not found'));
        }

        $dataLimitAdminIds = $this->getDataLimitAdminIds();
        if ($dataLimitAdminIds && !in_array($row[$this->dataLimitField], $dataLimitAdminIds)) {
            $this->error(__('You have no permission'));
        }

        if ($this->request->isPost()) {
            $data = $this->request->post();
            if (!$data) {
                $this->error(__('Parameter %s can not be empty', ['']));
            }

            if ($this->modelValidate) {
                try {
                    $validate = str_replace("\\model\\", "\\validate\\", get_class($this->model));
                    $validate = new $validate;
                    $validate->scene('edit')->check($data);
                } catch (Throwable $e) {
                    $this->error($e->getMessage());
                }
            }

            if ($this->auth->id == $data['id'] && $data['status'] == '0') {
                $this->error(__('Please use another administrator account to disable the current account!'));
            }
       
            // 更新密码
            if (isset($data['password']) && $data['password']) {
                $adminModel = new Admin();
                $res = $adminModel->resetPassword($data['id'], $data['password']);
                if (! $res) {
                    $this->error("更新密码失败");
                }
            }


            $data   = $this->excludeFields($data);
            $result = false;
            $this->model->startTrans();
            try {

         
                $update = [];
                $update['nickname'] = $data['nickname'];
                $update['avatar'] = $data['avatar'];
                $result = $row->save($update);
                $this->model->commit();
            } catch (Throwable $e) {
                $this->model->rollback();
                $this->error($e->getMessage());
            }
            if ($result !== false) {
                $this->success(__('Update successful'));
            } else {
                $this->error(__('No rows updated'));
            }
        }

        unset($row['salt'], $row['login_failure']);
        $row['password'] = '';
        $this->success('', [
            'row' => $row
        ]);
    }



    /**
     * 获取用户可选择下级用户的权限组
     */
    public function groupSelect()
    {
        $groupInfo = $this->auth->getGroups();
        $data = [];

        if (count($groupInfo) === 1) {
            if (isset($groupInfo[0]['group_id'])) {

                $groupId = $groupInfo[0]['group_id'];
                $data = config('group.select.'.$groupId);
            }
        }
        $this->success("", [ 'options' => $data]);
    }

    /**
     * 获取用户可见用户列表 (这个不用在添加用户中, 添加用户的逻辑是当前用户id就是pid)
     */
    public function memberSelect()
    {

    }
}
```

### 2.模型

```php
<?php

namespace app\admin\model;

use think\Model;

/**
 * Member
 */
class Member extends Model
{
    // 表名
    protected $name = 'admin';

    // 自动写入时间戳字段
    protected $autoWriteTimestamp = true;


    public function pidTable(): \think\model\relation\BelongsTo
    {
        return $this->belongsTo(\app\admin\model\Admin::class, 'pid', 'id');
    }
}
```

### 3.验证器

```php
<?php
namespace app\admin\validate;

use think\Validate;

class Member extends Validate
{
    protected $failException = true;

    protected $rule = [
        'username'  => 'require|regex:/^[a-zA-Z][a-zA-Z0-9_]{2,15}$/|unique:admin',
        'nickname'  => 'require',
        'password'  => 'require|regex:/^(?!.*[&<>"\'\n\r]).{6,32}$/',
        'group_id' => 'require',
    ];

    /**
     * 验证提示信息
     * @var array
     */
    protected $message = [
        'username.require' => '用户名不能为空',
        'username.regex' => '用户名必须以字母开头，只能包含字母、数字和下划线，长度在3-16之间',
        'username.unique' => '用户名已存在',
        'nickname.require' => '昵称不能为空',
        'password.require' => '密码不能为空',
        'password.regex' => '密码必须在6-32位之间，且不能包含特殊字符',
        'group_id.require' => '角色ID不能为空',
    ];

    /**
     * 字段描述
     */
    protected $field = [
        'username'  => '用户名',
        'nickname'  => '昵称',
        'password'  => '密码',
        'group_id' => '角色ID',
    ];

    /**
     * 验证场景
     */
    protected $scene = [
        'add' => ['username', 'nickname', 'password', 'group_id'],
        'edit' => ['nickname'],
    ];

    public function __construct()
    {
        parent::__construct();
    }
}
```

### 4.用户组配置 `config\group.php`

```php
<?php
return [
    'name'  => [
        1   => '超级管理组',
        2   => '一级管理员',
        3   => '二级管理员',
        4   => '三级管理员',
    ],
    'list'  => [
        ['id'=>2, 'title'=>'一级管理员'],
        ['id'=>3, 'title'=>'二级管理员'],
        ['id'=>4, 'title'=>'三级管理员'],
    ],
    'select'  => [
        // 1.超管可添加
        1   => [
            ['id'=>2, 'title'=>'一级管理员'],
            ['id'=>3, 'title'=>'二级管理员'],
            ['id'=>4, 'title'=>'三级管理员'],
        ],
        // 2.一级管理员可添加
        2   => [
            ['id'=>3, 'title'=>'二级管理员'],
            ['id'=>4, 'title'=>'三级管理员'],
        ],
        // 3.二级管理员可添加
        3   => [
            ['id'=>3, 'title'=>'二级管理员'],
            ['id'=>4, 'title'=>'三级管理员'],
        ],
        4   => [],
    ],

];
```

---
## 前端代码

### 1.列表页面

```html
<template>
    <div class="default-main ba-table-box">
        <el-alert class="ba-table-alert" v-if="baTable.table.remark" :title="baTable.table.remark" type="info" show-icon />

        <!-- 表格顶部菜单 -->
        <!-- 自定义按钮请使用插槽，甚至公共搜索也可以使用具名插槽渲染，参见文档 -->
        <TableHeader
            :buttons="['refresh', 'add', 'edit', 'delete', 'comSearch', 'quickSearch', 'columnDisplay']"
            :quick-search-placeholder="t('Quick search placeholder', { fields: t('member.quick Search Fields') })"
        ></TableHeader>

        <!-- 表格 -->
        <!-- 表格列有多种自定义渲染方式，比如自定义组件、具名插槽等，参见文档 -->
        <!-- 要使用 el-table 组件原有的属性，直接加在 Table 标签上即可 -->
        <Table ref="tableRef"></Table>

        <!-- 表单 -->
        <PopupForm />
        <EditForm />
    </div>
</template>

<script setup lang="ts">
import { ref, provide, onMounted } from 'vue'
import baTableClass from '/@/utils/baTable'
import { defaultOptButtons } from '/@/components/table'
import { baTableApi } from '/@/api/common'
import { useI18n } from 'vue-i18n'
import PopupForm from './popupForm.vue'
import EditForm from './editForm.vue'
import Table from '/@/components/table/index.vue'
import TableHeader from '/@/components/table/header/index.vue'

defineOptions({
    name: 'member',
})

const { t } = useI18n()
const tableRef = ref()
const optButtons: OptButton[] = defaultOptButtons(['edit'])

/**
 * baTable 内包含了表格的所有数据且数据具备响应性，然后通过 provide 注入给了后代组件
 */
const baTable = new baTableClass(
    new baTableApi('/admin/Member/'),
    {
        pk: 'id',
        column: [
            { type: 'selection', align: 'center', operator: false },
            // { label: t('member.id'), prop: 'id', align: 'center', width: 70, operator: false, sortable: 'custom' },
            { label: t('member.username'), prop: 'username', align: 'center', operatorPlaceholder: t('Fuzzy query'), operator: 'LIKE', sortable: false },
            { label: t('member.nickname'), prop: 'nickname', align: 'center', operatorPlaceholder: t('Fuzzy query'), operator: 'LIKE', sortable: false },
            { label: t('member.avatar'), prop: 'avatar', align: 'center', render: 'image', operator: false },
            { label: t('member.status'), prop: 'status', align: 'center', render: 'tag', operator: false, sortable: false, replaceValue: { '0': t('member.status 0'), '1': t('member.status 1') } },
            { label: "上级用户", prop: 'pidTable.username', align: 'center', operatorPlaceholder: t('Fuzzy query'), render: 'tags', operator: 'LIKE' },
            { label: t('member.pid'), prop: 'pid', align: 'center', operator: 'eq', show:false },
            { label: t('member.create_time'), prop: 'create_time', align: 'center', render: 'datetime', operator: false, sortable: 'custom', width: 160, timeFormat: 'yyyy-mm-dd hh:MM:ss' },
            { label: t('Operate'), align: 'center', width: 100, render: 'buttons', buttons: optButtons, operator: false },
        ],
        dblClickNotEditColumn: ['all'],
    },
    {
        defaultItems: { status: '1' },
    }
)

provide('baTable', baTable)

onMounted(() => {
    baTable.table.ref = tableRef.value
    baTable.mount()
    baTable.getIndex()?.then(() => {
        baTable.initSort()
        baTable.dragSort()
    })
})
</script>

<style scoped lang="scss"></style>
```

### 2.添加页面(这里将add和edit拆分, 并且重写了后台的控制器方法)

```html
<template>
    <!-- 对话框表单 -->
    <!-- 建议使用 Prettier 格式化代码 -->
    <!-- el-form 内可以混用 el-form-item、FormItem、ba-input 等输入组件 -->
    <el-dialog
        class="ba-operate-dialog"
        :close-on-click-modal="false"
        :model-value="['Add'].includes(baTable.form.operate!)"
        @close="baTable.toggleForm"
        width="50%"
    >
        <template #header>
            <div class="title" v-drag="['.ba-operate-dialog', '.el-dialog__header']" v-zoom="'.ba-operate-dialog'">
                {{ baTable.form.operate ? t(baTable.form.operate) : '' }}
            </div>
        </template>
        <el-scrollbar v-loading="baTable.form.loading" class="ba-table-form-scrollbar">
            <div
                class="ba-operate-form"
                :class="'ba-' + baTable.form.operate + '-form'"
                :style="'width: calc(100% - ' + baTable.form.labelWidth! / 2 + 'px)'"
            >
                <el-form
                    v-if="!baTable.form.loading"
                    ref="formRef"
                    @submit.prevent=""
                    @keyup.enter="baTable.onSubmit(formRef)"
                    :model="baTable.form.items"
                    label-position="right"
                    :label-width="baTable.form.labelWidth + 'px'"
                    :rules="rules"
                >
                    <FormItem :label="t('member.username')" type="string" v-model="baTable.form.items!.username" prop="username" placeholder="由英文和大小写字母组成, 最少6位" />
                    <FormItem :label="t('member.nickname')" type="string" v-model="baTable.form.items!.nickname" prop="nickname" placeholder="可输入中文和英文,数字" />
                    <FormItem :label="t('member.avatar')" type="image" v-model="baTable.form.items!.avatar" prop="avatar" />
                    <FormItem :label="t('member.password')" type="string" v-model="baTable.form.items!.password" prop="password" :placeholder="t('Please input field', { field: t('member.password') })" />
                    <FormItem :label="t('member.status')" type="radio" v-model="baTable.form.items!.status" prop="status" :data="{ content: { '0': t('member.status 0'), '1': t('member.status 1') } }" :placeholder="t('Please select field', { field: t('member.status') })" />
                    <FormItem label="角色" 
                        type="remoteSelect" 
                        v-model="baTable.form.items!.group_id" 
                        prop="group_id" 
                        :input-attr="{ pk: 'id', field: 'title', 'remote-url': groupSelect }"

                    />
                    
                </el-form>
            </div>
        </el-scrollbar>
        <template #footer>
            <div :style="'width: calc(100% - ' + baTable.form.labelWidth! / 1.8 + 'px)'">
                <el-button @click="baTable.toggleForm()">{{ t('Cancel') }}</el-button>
                <el-button v-blur :loading="baTable.form.submitLoading" @click="baTable.onSubmit(formRef)" type="primary">
                    {{ baTable.form.operateIds && baTable.form.operateIds.length > 1 ? t('Save and edit next item') : t('Save') }}
                </el-button>
            </div>
        </template>
    </el-dialog>
</template>

<script setup lang="ts">
import { reactive, ref, inject } from 'vue'
import { useI18n } from 'vue-i18n'
import type baTableClass from '/@/utils/baTable'
import FormItem from '/@/components/formItem/index.vue'
import type { FormInstance, FormItemRule } from 'element-plus'
import { buildValidatorData } from '/@/utils/validate'
import roleConfig from '/@/config/system/roleConfig'
import { useAdminInfo } from '/@/stores/adminInfo'
import { groupSelect } from '/@/api/controllerUrls' 


const formRef = ref<FormInstance>()
const baTable = inject('baTable') as baTableClass

const { t } = useI18n()

const rules: Partial<Record<string, FormItemRule[]>> = reactive({
    last_login_time: [buildValidatorData({ name: 'number', title: t('member.last_login_time') })],
    update_time: [buildValidatorData({ name: 'date', title: t('member.update_time') })],
    create_time: [buildValidatorData({ name: 'date', title: t('member.create_time') })],
    // pid: [buildValidatorData({ name: 'number', title: t('member.pid') })],
})


</script>

<style scoped lang="scss"></style>
```

### 3.编辑页面

```html
<template>
    <!-- 对话框表单 -->
    <!-- 建议使用 Prettier 格式化代码 -->
    <!-- el-form 内可以混用 el-form-item、FormItem、ba-input 等输入组件 -->
    <el-dialog
        class="ba-operate-dialog"
        :close-on-click-modal="false"
        :model-value="['Edit'].includes(baTable.form.operate!)"
        @close="baTable.toggleForm"
        width="50%"
    >
        <template #header>
            <div class="title" v-drag="['.ba-operate-dialog', '.el-dialog__header']" v-zoom="'.ba-operate-dialog'">
                {{ baTable.form.operate ? t(baTable.form.operate) : '' }}
            </div>
        </template>
        <el-scrollbar v-loading="baTable.form.loading" class="ba-table-form-scrollbar">
            <div
                class="ba-operate-form"
                :class="'ba-' + baTable.form.operate + '-form'"
                :style="'width: calc(100% - ' + baTable.form.labelWidth! / 2 + 'px)'"
            >
                <el-form
                    v-if="!baTable.form.loading"
                    ref="formRef"
                    @submit.prevent=""
                    @keyup.enter="baTable.onSubmit(formRef)"
                    :model="baTable.form.items"
                    label-position="right"
                    :label-width="baTable.form.labelWidth + 'px'"
                    :rules="rules"
                >
                    <FormItem :label="t('member.nickname')" type="string" v-model="baTable.form.items!.nickname" prop="nickname" placeholder="可输入中文和英文,数字" />
                    <FormItem :label="t('member.avatar')" type="image" v-model="baTable.form.items!.avatar" prop="avatar" />
                    <FormItem :label="t('member.password')" type="string" v-model="baTable.form.items!.password" prop="password" :placeholder="t('Please input field', { field: t('member.password') })" />
                    <FormItem :label="t('member.status')" type="radio" v-model="baTable.form.items!.status" prop="status" :data="{ content: { '0': t('member.status 0'), '1': t('member.status 1') } }" :placeholder="t('Please select field', { field: t('member.status') })" />
                </el-form>
            </div>
        </el-scrollbar>
        <template #footer>
            <div :style="'width: calc(100% - ' + baTable.form.labelWidth! / 1.8 + 'px)'">
                <el-button @click="baTable.toggleForm()">{{ t('Cancel') }}</el-button>
                <el-button v-blur :loading="baTable.form.submitLoading" @click="baTable.onSubmit(formRef)" type="primary">
                    {{ baTable.form.operateIds && baTable.form.operateIds.length > 1 ? t('Save and edit next item') : t('Save') }}
                </el-button>
            </div>
        </template>
    </el-dialog>
</template>

<script setup lang="ts">
import { reactive, ref, inject } from 'vue'
import { useI18n } from 'vue-i18n'
import type baTableClass from '/@/utils/baTable'
import FormItem from '/@/components/formItem/index.vue'
import type { FormInstance, FormItemRule } from 'element-plus'
import { buildValidatorData } from '/@/utils/validate'
import roleConfig from '/@/config/system/roleConfig'
import { useAdminInfo } from '/@/stores/adminInfo'
import { groupSelect } from '/@/api/controllerUrls' 


const formRef = ref<FormInstance>()
const baTable = inject('baTable') as baTableClass

const { t } = useI18n()

const rules: Partial<Record<string, FormItemRule[]>> = reactive({
    last_login_time: [buildValidatorData({ name: 'number', title: t('member.last_login_time') })],
    update_time: [buildValidatorData({ name: 'date', title: t('member.update_time') })],
    create_time: [buildValidatorData({ name: 'date', title: t('member.create_time') })],
    // pid: [buildValidatorData({ name: 'number', title: t('member.pid') })],
})


</script>

<style scoped lang="scss"></style>
```
### 4.前端路由组件 `web\src\api\controllerUrls.ts`

```ts
/**
 * 通常将表格的控制器URL定义在此处

此文件是 1.0 版本的, 2.0版本不见此文件, 但是文档中还有
export const authMenu = '/admin/auth.menu/'
export const authAdmin = '/admin/auth.admin/'
export const authAdminLog = '/admin/auth.adminLog/'
export const authGroup = '/admin/auth.group/'
export const routineAttachment = '/admin/routine.attachment/'
export const userUser = '/admin/user.user/'
export const userGroup = '/admin/user.group/'
export const userRule = '/admin/user.rule/'
export const userScoreLog = '/admin/user.scoreLog/'
export const userMoneyLog = '/admin/user.moneyLog/'
export const securityDataRecycle = '/admin/security.dataRecycle/'
export const securityDataRecycleLog = '/admin/security.dataRecycleLog/'
export const securitySensitiveData = '/admin/security.sensitiveData/'
export const securitySensitiveDataLog = '/admin/security.sensitiveDataLog/'

 */

import { useAdminInfo } from "../stores/adminInfo"

export const goodsCategory = '/admin/goodsCategory/'
export const userUser = '/admin/user.user/'
// 用户组远程下拉
export const groupSelect = '/admin/member/groupSelect/'
```