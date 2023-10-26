# 二维码问题

- QrCode二维码的使用

教程
```
https://blog.csdn.net/G_ing/article/details/128162720
```

### 1. 安装

```
pnpm install --save-dev @types/qrcodejs2-fix
```

## 2. 模板中

```html
<div>
    <div id="qrCodeBox"></div>
    <button @click="getQrCode()">获取二维码</button>
</div>
```

### 3. 使用

```ts
import QRCode from 'qrcodejs2-fix'; //在需要使用的vue文件中导入即可, 这里IDE会有提示, 但不影响使用

const getQrCode = () => {
    const qrCodeBox = document.getElementById("qrCodeBox");
    if (qrCodeBox) {
        qrCodeBox.innerHTML = ""; //清空该元素内内容

        getQrcode().then((res) => {
            let url = res.data.url
            new QRCode(document.getElementById("qrCodeBox"), {
                text: url,
                width: 200, //二维码宽
                height: 200, //二维码高
            });
        })
        
    } 
};
```
### 4.api请求 `web\src\api\public\system.ts`

```ts
export function getQrcode() {
    return createAxios({
        url: '/admin/dashboard/getQrcode',
        method: 'get',
    })
}
```

### 5.控制器中 `app\admin\controller\Dashboard.php`

```php
// 获取qrcode
public function getQrcode()
{
    $this->success('', ['url' => 'https://www.qq.com']);
}
```