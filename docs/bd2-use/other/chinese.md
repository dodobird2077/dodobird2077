# 允许输入中文用户名

## 修改的文件

```bash
$ git status
...
Changes not staged for commit:
        modified:   app/admin/validate/Member.php
        modified:   web/src/lang/common/zh-cn/validate.ts
        modified:   web/src/utils/validate.ts
```

## 1.后端验证器(自定义的) `app/admin/validate/Member.php`

```php
class Member extends Validate
{
    protected $failException = true;

    protected $rule = [
        //'username'  => 'require|regex:/^[a-zA-Z][a-zA-Z0-9_]{2,15}$/|unique:admin',
        'username'  => 'require|chsDash|length:3,16|unique:admin',
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
        // 'username.regex' => '用户名必须以字母开头，只能包含字母、数字和下划线，长度在3-16之间',
        'username.chsDash' => '用户名必须由汉字、字母、数字和下划线组成',
        'username.length' => '用户名长度必须在3-16之间',
        
        'username.unique' => '用户名已存在',
        'nickname.require' => '昵称不能为空',
        'password.require' => '密码不能为空',
        'password.regex' => '密码必须在6-32位之间，且不能包含特殊字符',
        'group_id.require' => '角色ID不能为空',
    ];
    // 省略
}
```


## 2.语言包, 登录框提示词 `web/src/lang/common/zh-cn/validate.ts`

```ts
export default {
    // 省略
    // 'Please enter the correct account': '要求3到15位，字母开头且只含字母、数字、下划线',
    'Please enter the correct account': '要求3到15位的合法用户名,不能包含特殊字符',
}
```


## 3.前端登录验证器 `web/src/utils/validate.ts` 30行左右

```ts

/**
 * 账户名验证
 */
// export function validatorAccount(rule: any, val: string, callback: Function) {
//     if (!val) {
//         return callback()
//     }
//     if (!/^[a-zA-Z][a-zA-Z0-9_]{2,15}$/.test(val)) {
//         return callback(new Error(i18n.global.t('validate.Please enter the correct account')))
//     }
//     return callback()
// }
// 改为允许中文
export function validatorAccount(rule: any, val: string, callback: Function) {
    if (!val) {
        return callback()
    }
    // 允许输入中文字符，并且限制总字符数
    if (!/^[\u4e00-\u9fa5a-zA-Z0-9_]{3,16}$/.test(val)) {
        return callback(new Error(i18n.global.t('validate.Please enter the correct account')))
    }
    return callback()
}

```