# 常见问题

## 1.如何更换npm 源
```
待完善
```

## 2.如何设置vsCode高亮
- 使用 `Volar` 插件

## 3.如何解决vsCode语法报错问题

1. `Ctrl+shift+P` , 输入 `Volar`, 然后选择`reload project`, 然后重启vscode即可

## 4.文件上传, 表单跨域


## 5.关于线上, 后端路由index.php报错的问题

 例如导入的url

 ```ts
export const importGoods = url + '/index.php/admin/goods/importGoods?batoken=' + token // 导入
 ```

后端报错提示: Index/php 不存在, 就是把index.php当成了控制器和方法

`解决方法` 去掉index.php, 增加 `server=1&`

 ```ts
export const importGoods = url + '/admin/goods/importGoods?server=1&batoken=' + token // 导入
 ```
