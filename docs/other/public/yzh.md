# 部标JT/T808协议接入平台


**简介**:部标JT/T808协议接入平台


**HOST**:http://127.0.0.1:8080


**联系人**:问题交流群：906230542


**Version**:1.0.0


**接口路径**:/v3/api-docs

```
https://gitee.com/yezhihao/jt808-server
```


[TOC]






# other-controller


## 808协议分析工具


**接口地址**:`/message/explain`


**请求方式**:`GET`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`*/*`


**接口描述**:


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|hex|16进制报文|query|true|string||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK||


**响应参数**:


暂无


**响应示例**:
```javascript

```


## 808协议分析工具


**接口地址**:`/message/explain`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`*/*`


**接口描述**:


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|hex|16进制报文|query|true|string||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK||


**响应参数**:


暂无


**响应示例**:
```javascript

```


## 设备监控


**接口地址**:`/device/sse`


**请求方式**:`GET`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`text/event-stream`


**接口描述**:


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|clientId||query|true|string||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK||


**响应参数**:


暂无


**响应示例**:
```javascript

```


## 设备订阅


**接口地址**:`/device/sse`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`text/plain`


**接口描述**:


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|clientId||query|true|string||
|sub||query|true|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK||


**响应参数**:


暂无


**响应示例**:
```javascript

```


## 原始消息发送


**接口地址**:`/device/raw`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`*/*`


**接口描述**:


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|clientId|终端手机号|query|true|string||
|message|16进制报文|query|true|string||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK||


**响应参数**:


暂无


**响应示例**:
```javascript

```


## 修改日志级别


**接口地址**:`/logger`


**请求方式**:`GET`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`*/*`


**接口描述**:


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|level|可用值:TRACE,DEBUG,INFO,WARN,ERROR|query|true|string||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK||


**响应参数**:


暂无


**响应示例**:
```javascript

```


## 获得当前所有在线设备信息


**接口地址**:`/device/option`


**请求方式**:`GET`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`*/*`


**接口描述**:


**请求参数**:


暂无


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultCollectionDeviceDO|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data|响应数据|array|DeviceDO|
|&emsp;&emsp;deviceId|设备id|string||
|&emsp;&emsp;mobileNo|设备手机号|string||
|&emsp;&emsp;plateNo|车牌号|string||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;location||T0200|T0200|
|&emsp;&emsp;&emsp;&emsp;warnBit|报警标志|integer||
|&emsp;&emsp;&emsp;&emsp;statusBit|状态|integer||
|&emsp;&emsp;&emsp;&emsp;latitude|纬度|integer||
|&emsp;&emsp;&emsp;&emsp;longitude|经度|integer||
|&emsp;&emsp;&emsp;&emsp;altitude|高程(米)|integer||
|&emsp;&emsp;&emsp;&emsp;speed|速度(1/10公里每小时)|integer||
|&emsp;&emsp;&emsp;&emsp;direction|方向|integer||
|&emsp;&emsp;&emsp;&emsp;deviceTime|时间(YYMMDDHHMMSS)|string||
|&emsp;&emsp;&emsp;&emsp;lng||number||
|&emsp;&emsp;&emsp;&emsp;lat||number||
|&emsp;&emsp;&emsp;&emsp;speedKph||number||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": [
		{
			"deviceId": "",
			"mobileNo": "",
			"plateNo": "",
			"location": {
				"warnBit": 0,
				"statusBit": 0,
				"latitude": 0,
				"longitude": 0,
				"altitude": 0,
				"speed": 0,
				"direction": 0,
				"deviceTime": "",
				"lng": 0,
				"lat": 0,
				"speedKph": 0
			}
		}
	],
	"success": true
}
```


## 终端实时信息查询


**接口地址**:`/device/all`


**请求方式**:`GET`


**请求数据类型**:`application/x-www-form-urlencoded`


**响应数据类型**:`*/*`


**接口描述**:


**请求参数**:


暂无


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultCollectionSession|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data|响应数据|array|Session|
|&emsp;&emsp;udp||boolean||
|&emsp;&emsp;remoteAddressStr||string||
|&emsp;&emsp;creationTime||integer(int64)||
|&emsp;&emsp;lastAccessedTime||integer(int64)||
|&emsp;&emsp;attributes||object||
|&emsp;&emsp;clientId||string||
|&emsp;&emsp;registered||boolean||
|&emsp;&emsp;id||string||
|&emsp;&emsp;attributeNames||array|object|
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": [
		{
			"udp": true,
			"remoteAddressStr": "",
			"creationTime": 0,
			"lastAccessedTime": 0,
			"attributes": {},
			"clientId": "",
			"registered": true,
			"id": "",
			"attributeNames": []
		}
	],
	"success": true
}
```


# jt-1078-controller


## 9306 云台变倍控制


**接口地址**:`/device/9306`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "param": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9302|T9302|body|true|T9302|T9302|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;param|参数(0.调大 1.调小)|(0.停止 1.启动)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9305 红外补光控制


**接口地址**:`/device/9305`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "param": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9302|T9302|body|true|T9302|T9302|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;param|参数(0.调大 1.调小)|(0.停止 1.启动)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9304 云台雨刷控制


**接口地址**:`/device/9304`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "param": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9302|T9302|body|true|T9302|T9302|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;param|参数(0.调大 1.调小)|(0.停止 1.启动)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9303 云台调整光圈控制


**接口地址**:`/device/9303`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "param": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9302|T9302|body|true|T9302|T9302|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;param|参数(0.调大 1.调小)|(0.停止 1.启动)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9302 云台调整焦距控制


**接口地址**:`/device/9302`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "param": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9302|T9302|body|true|T9302|T9302|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;param|参数(0.调大 1.调小)|(0.停止 1.启动)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9301 云台旋转


**接口地址**:`/device/9301`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "param1": 0,
  "param2": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9301|T9301|body|true|T9301|T9301|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;param1|方向：0.停止 1.上 2.下 3.左 4.右||true|integer(int32)||
|&emsp;&emsp;param2|速度(0~255)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9208 报警附件上传指令(苏标)


**接口地址**:`/device/9208`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "ip": "",
  "tcpPort": 0,
  "udpPort": 0,
  "deviceId": "",
  "dateTime": "",
  "sequenceNo": 0,
  "fileTotal": 0,
  "platformAlarmId": "",
  "reserves": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9208|T9208|body|true|T9208|T9208|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;ip|服务器IP地址||true|string||
|&emsp;&emsp;tcpPort|TCP端口||true|integer(int32)||
|&emsp;&emsp;udpPort|UDP端口||true|integer(int32)||
|&emsp;&emsp;deviceId|终端ID||true|string||
|&emsp;&emsp;dateTime|时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;sequenceNo|序号(同一时间点报警的序号，从0循环累加)||true|integer(int32)||
|&emsp;&emsp;fileTotal|附件数量||true|integer(int32)||
|&emsp;&emsp;reserved|预留||false|integer(int32)||
|&emsp;&emsp;platformAlarmId|报警编号||true|string||
|&emsp;&emsp;reserves|预留||true|array|string(byte)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9207 文件上传控制


**接口地址**:`/device/9207`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "responseSerialNo": 0,
  "command": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9207|T9207|body|true|T9207|T9207|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;responseSerialNo|应答流水号||true|integer(int32)||
|&emsp;&emsp;command|上传控制：0.暂停 1.继续 2.取消||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9206 文件上传指令


**接口地址**:`/device/9206`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "ip": "",
  "port": 0,
  "username": "",
  "password": "",
  "path": "",
  "channelNo": 0,
  "startTime": "",
  "endTime": "",
  "warnBit1": 0,
  "warnBit2": 0,
  "mediaType": 0,
  "streamType": 0,
  "storageType": 0,
  "condition": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9206|T9206|body|true|T9206|T9206|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;ip|服务器地址||true|string||
|&emsp;&emsp;port|端口||true|integer(int32)||
|&emsp;&emsp;username|用户名||true|string||
|&emsp;&emsp;password|密码||true|string||
|&emsp;&emsp;path|文件上传路径||true|string||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;startTime|开始时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;endTime|结束时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)||true|integer(int32)||
|&emsp;&emsp;warnBit2|报警标志32~63||true|integer(int32)||
|&emsp;&emsp;mediaType|音视频资源类型：0.音视频 1.音频 2.视频 3.视频或音视频||true|integer(int32)||
|&emsp;&emsp;streamType|码流类型：0.所有码流 1.主码流 2.子码流||true|integer(int32)||
|&emsp;&emsp;storageType|存储位置：0.所有存储器 1.主存储器 2.灾备存储器||true|integer(int32)||
|&emsp;&emsp;condition|任务执行条件(用bit位表示)：[0]WIFI下可下载 [1]LAN连接时可下载 [2]3G/4G连接时可下载||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9205 查询资源列表


**接口地址**:`/device/9205`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "startTime": "",
  "endTime": "",
  "warnBit1": 0,
  "warnBit2": 0,
  "mediaType": 0,
  "streamType": 0,
  "storageType": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9205|T9205|body|true|T9205|T9205|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;startTime|开始时间(YYMMDDHHMMSS,全0表示无起始时间)||true|string||
|&emsp;&emsp;endTime|结束时间(YYMMDDHHMMSS,全0表示无终止时间)||true|string||
|&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)||true|integer(int32)||
|&emsp;&emsp;warnBit2|报警标志32~63||true|integer(int32)||
|&emsp;&emsp;mediaType|音视频资源类型：0.音视频 1.音频 2.视频 3.视频或音视频||true|integer(int32)||
|&emsp;&emsp;streamType|码流类型：0.所有码流 1.主码流 2.子码流||true|integer(int32)||
|&emsp;&emsp;storageType|存储器类型：0.所有存储器 1.主存储器 2.灾备存储器||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT1205|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T1205|T1205|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;items|音视频资源列表|array|Item|
|&emsp;&emsp;&emsp;&emsp;channelNo|逻辑通道号|integer||
|&emsp;&emsp;&emsp;&emsp;startTime|开始时间|string||
|&emsp;&emsp;&emsp;&emsp;endTime|结束时间|string||
|&emsp;&emsp;&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)|integer||
|&emsp;&emsp;&emsp;&emsp;warnBit2|报警标志32~63|integer||
|&emsp;&emsp;&emsp;&emsp;mediaType|音视频资源类型|integer||
|&emsp;&emsp;&emsp;&emsp;streamType|码流类型|integer||
|&emsp;&emsp;&emsp;&emsp;storageType|存储器类型|integer||
|&emsp;&emsp;&emsp;&emsp;size|文件大小|integer||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"items": [
			{
				"channelNo": 0,
				"startTime": "",
				"endTime": "",
				"warnBit1": 0,
				"warnBit2": 0,
				"mediaType": 0,
				"streamType": 0,
				"storageType": 0,
				"size": 0
			}
		]
	}
}
```


## 9202 平台下发远程录像回放控制


**接口地址**:`/device/9202`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "playbackMode": 0,
  "playbackSpeed": 0,
  "playbackTime": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9202|T9202|body|true|T9202|T9202|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;playbackMode|回放控制：0.开始回放 1.暂停回放 2.结束回放 3.快进回放 4.关键帧快退回放 5.拖动回放 6.关键帧播放||true|integer(int32)||
|&emsp;&emsp;playbackSpeed|快进或快退倍数：0.无效 1.1倍 2.2倍 3.4倍 4.8倍 5.16倍 (回放控制为3和4时,此字段内容有效,否则置0)||true|integer(int32)||
|&emsp;&emsp;playbackTime|拖动回放位置(YYMMDDHHMMSS,回放控制为5时,此字段有效)||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9201 平台下发远程录像回放请求


**接口地址**:`/device/9201`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "ip": "",
  "tcpPort": 0,
  "udpPort": 0,
  "channelNo": 0,
  "mediaType": 0,
  "streamType": 0,
  "storageType": 0,
  "playbackMode": 0,
  "playbackSpeed": 0,
  "startTime": "",
  "endTime": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9201|T9201|body|true|T9201|T9201|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;ip|服务器IP地址||true|string||
|&emsp;&emsp;tcpPort|实时视频服务器TCP端口号||true|integer(int32)||
|&emsp;&emsp;udpPort|实时视频服务器UDP端口号||true|integer(int32)||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;mediaType|音视频资源类型：0.音视频 1.音频 2.视频 3.视频或音视频||true|integer(int32)||
|&emsp;&emsp;streamType|码流类型：0.所有码流 1.主码流 2.子码流(如果此通道只传输音频,此字段置0)||true|integer(int32)||
|&emsp;&emsp;storageType|存储器类型：0.所有存储器 1.主存储器 2.灾备存储器||true|integer(int32)||
|&emsp;&emsp;playbackMode|回放方式：0.正常回放 1.快进回放 2.关键帧快退回放 3.关键帧播放 4.单帧上传||true|integer(int32)||
|&emsp;&emsp;playbackSpeed|快进或快退倍数：0.无效 1.1倍 2.2倍 3.4倍 4.8倍 5.16倍 (回放控制为1和2时,此字段内容有效,否则置0)||true|integer(int32)||
|&emsp;&emsp;startTime|开始时间(YYMMDDHHMMSS,回放方式为4时,该字段表示单帧上传时间)||true|string||
|&emsp;&emsp;endTime|结束时间(YYMMDDHHMMSS,回放方式为4时,该字段无效,为0表示一直回放)||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT1205|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T1205|T1205|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;items|音视频资源列表|array|Item|
|&emsp;&emsp;&emsp;&emsp;channelNo|逻辑通道号|integer||
|&emsp;&emsp;&emsp;&emsp;startTime|开始时间|string||
|&emsp;&emsp;&emsp;&emsp;endTime|结束时间|string||
|&emsp;&emsp;&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)|integer||
|&emsp;&emsp;&emsp;&emsp;warnBit2|报警标志32~63|integer||
|&emsp;&emsp;&emsp;&emsp;mediaType|音视频资源类型|integer||
|&emsp;&emsp;&emsp;&emsp;streamType|码流类型|integer||
|&emsp;&emsp;&emsp;&emsp;storageType|存储器类型|integer||
|&emsp;&emsp;&emsp;&emsp;size|文件大小|integer||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"items": [
			{
				"channelNo": 0,
				"startTime": "",
				"endTime": "",
				"warnBit1": 0,
				"warnBit2": 0,
				"mediaType": 0,
				"streamType": 0,
				"storageType": 0,
				"size": 0
			}
		]
	}
}
```


## 9102 音视频实时传输控制


**接口地址**:`/device/9102`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelNo": 0,
  "command": 0,
  "closeType": 0,
  "streamType": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9102|T9102|body|true|T9102|T9102|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;command|控制指令： 0.关闭音视频传输指令 1.切换码流(增加暂停和继续) 2.暂停该通道所有流的发送 3.恢复暂停前流的发送,与暂停前的流类型一致 4.关闭双向对讲||true|integer(int32)||
|&emsp;&emsp;closeType|关闭音视频类型： 0.关闭该通道有关的音视频数据 1.只关闭该通道有关的音频,保留该通道有关的视频 2.只关闭该通道有关的视频,保留该通道有关的音频||true|integer(int32)||
|&emsp;&emsp;streamType|切换码流类型：0.主码流 1.子码流||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9101 实时音视频传输请求


**接口地址**:`/device/9101`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "ip": "",
  "tcpPort": 0,
  "udpPort": 0,
  "channelNo": 0,
  "mediaType": 0,
  "streamType": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t9101|T9101|body|true|T9101|T9101|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;ip|服务器IP地址||true|string||
|&emsp;&emsp;tcpPort|实时视频服务器TCP端口号||true|integer(int32)||
|&emsp;&emsp;udpPort|实时视频服务器UDP端口号||true|integer(int32)||
|&emsp;&emsp;channelNo|逻辑通道号||true|integer(int32)||
|&emsp;&emsp;mediaType|数据类型：0.音视频 1.视频 2.双向对讲 3.监听 4.中心广播 5.透传||true|integer(int32)||
|&emsp;&emsp;streamType|码流类型：0.主码流 1.子码流||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 9003 查询终端音视频属性


**接口地址**:`/device/9003`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT1003|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T1003|T1003|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;audioFormat|输入音频编码方式|integer(int32)||
|&emsp;&emsp;audioChannels|输入音频声道数|integer(int32)||
|&emsp;&emsp;audioSamplingRate|输入音频采样率：0.8kHz 1.22.05kHz 2.44.1kHz 3.48kHz|integer(int32)||
|&emsp;&emsp;audioBitDepth|输入音频采样位数：0.8位 1.16位 2.32位|integer(int32)||
|&emsp;&emsp;audioFrameLength|音频帧长度|integer(int32)||
|&emsp;&emsp;audioSupport|是否支持音频输出|integer(int32)||
|&emsp;&emsp;videoFormat|视频编码方式|integer(int32)||
|&emsp;&emsp;maxAudioChannels|终端支持的最大音频物理通道|integer(int32)||
|&emsp;&emsp;maxVideoChannels|终端支持的最大视频物理通道|integer(int32)||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"audioFormat": 0,
		"audioChannels": 0,
		"audioSamplingRate": 0,
		"audioBitDepth": 0,
		"audioFrameLength": 0,
		"audioSupport": 0,
		"videoFormat": 0,
		"maxAudioChannels": 0,
		"maxVideoChannels": 0
	}
}
```


# jt-808-controller


## 8A00 平台RSA公钥


**接口地址**:`/device/8A00`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "e": 0,
  "n": [],
  "getnBase64": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t0A00_8A00|T0A00_8A00|body|true|T0A00_8A00|T0A00_8A00|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;e|RSA公钥{e,n}中的e||true|integer(int32)||
|&emsp;&emsp;n|RSA公钥{e,n}中的n||true|array|string(byte)|
|&emsp;&emsp;getnBase64|n(BASE64编码)||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0A00_8A00|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0A00_8A00|T0A00_8A00|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;e|RSA公钥{e,n}中的e|integer(int32)||
|&emsp;&emsp;n|RSA公钥{e,n}中的n|array|string(byte)|
|&emsp;&emsp;getnBase64|n(BASE64编码)|string||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"e": 0,
		"n": [],
		"getnBase64": ""
	}
}
```


## 8900 数据下行透传


**接口地址**:`/device/8900`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "message": {
    "key": 0,
    "value": {}
  },
  "peripheralStatus": {
    "items": [
      {
        "channelNo": 0,
        "startTime": "",
        "endTime": "",
        "warnBit1": 0,
        "warnBit2": 0,
        "mediaType": 0,
        "streamType": 0,
        "storageType": 0,
        "size": 0
      }
    ]
  },
  "peripheralSystem": {
    "items": [
      {
        "channelNo": 0,
        "startTime": "",
        "endTime": "",
        "warnBit1": 0,
        "warnBit2": 0,
        "mediaType": 0,
        "streamType": 0,
        "storageType": 0,
        "size": 0
      }
    ]
  },
  "bodyLength": 0,
  "encryption": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8900|T8900|body|true|T8900|T8900|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;message|||false|KeyValuePairIntegerObject|KeyValuePairIntegerObject|
|&emsp;&emsp;&emsp;&emsp;key|||false|integer||
|&emsp;&emsp;&emsp;&emsp;value|||false|object||
|&emsp;&emsp;peripheralStatus|状态查询(外设状态信息：外设工作状态、设备报警信息)||false|PeripheralStatus|PeripheralStatus|
|&emsp;&emsp;&emsp;&emsp;items|||false|array|Item|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;channelNo|逻辑通道号||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;startTime|开始时间||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;endTime|结束时间||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;warnBit2|报警标志32~63||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;mediaType|音视频资源类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;streamType|码流类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageType|存储器类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;size|文件大小||false|integer||
|&emsp;&emsp;peripheralSystem|信息查询(外设传感器的基本信息：公司信息、产品代码、版本号、外设ID、客户代码)||false|PeripheralSystem|PeripheralSystem|
|&emsp;&emsp;&emsp;&emsp;items|||false|array|Item|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;channelNo|逻辑通道号||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;startTime|开始时间||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;endTime|结束时间||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;warnBit2|报警标志32~63||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;mediaType|音视频资源类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;streamType|码流类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageType|存储器类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;size|文件大小||false|integer||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8805 单条存储多媒体数据检索上传命令


**接口地址**:`/device/8805`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": 0,
  "delete": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8805|T8805|body|true|T8805|T8805|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|多媒体ID(大于0)||true|integer(int32)||
|&emsp;&emsp;delete|删除标志：0.保留 1.删除 ||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8804 录音开始命令


**接口地址**:`/device/8804`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "command": 0,
  "time": 0,
  "save": 0,
  "audioSamplingRate": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8804|T8804|body|true|T8804|T8804|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;command|录音命令：0.停止录音 1.开始录音||true|integer(int32)||
|&emsp;&emsp;time|录音时间(秒) 0.表示一直录音||true|integer(int32)||
|&emsp;&emsp;save|保存标志：0.实时上传 1.保存||true|integer(int32)||
|&emsp;&emsp;audioSamplingRate|音频采样率：0.8K 1.11K 2.23K 3.32K 其他保留||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8803 存储多媒体数据上传


**接口地址**:`/device/8803`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "channelId": 0,
  "event": 0,
  "startTime": "",
  "endTime": "",
  "delete": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8803|T8803|body|true|T8803|T8803|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|多媒体类型：0.图像 1.音频 2.视频 ||true|integer(int32)||
|&emsp;&emsp;channelId|通道ID||true|integer(int32)||
|&emsp;&emsp;event|事件项编码：0.平台下发指令 1.定时动作 2.抢劫报警触发 3.碰撞侧翻报警触发 其他保留||true|integer(int32)||
|&emsp;&emsp;startTime|起始时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;endTime|结束时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;delete|删除标志：0.保留 1.删除 ||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8802 存储多媒体数据检索


**接口地址**:`/device/8802`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "channelId": 0,
  "event": 0,
  "startTime": "",
  "endTime": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8802|T8802|body|true|T8802|T8802|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|多媒体类型：0.图像 1.音频 2.视频 ||true|integer(int32)||
|&emsp;&emsp;channelId|通道ID(0表示检索该媒体类型的所有通道)||true|integer(int32)||
|&emsp;&emsp;event|事件项编码：0.平台下发指令 1.定时动作 2.抢劫报警触发 3.碰撞侧翻报警触发 其他保留||true|integer(int32)||
|&emsp;&emsp;startTime|起始时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;endTime|结束时间(YYMMDDHHMMSS)||true|string(date-time)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0802|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0802|T0802|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;items|检索项|array|Item|
|&emsp;&emsp;&emsp;&emsp;channelNo|逻辑通道号|integer||
|&emsp;&emsp;&emsp;&emsp;startTime|开始时间|string||
|&emsp;&emsp;&emsp;&emsp;endTime|结束时间|string||
|&emsp;&emsp;&emsp;&emsp;warnBit1|报警标志0~31(参考808协议文档报警标志位定义)|integer||
|&emsp;&emsp;&emsp;&emsp;warnBit2|报警标志32~63|integer||
|&emsp;&emsp;&emsp;&emsp;mediaType|音视频资源类型|integer||
|&emsp;&emsp;&emsp;&emsp;streamType|码流类型|integer||
|&emsp;&emsp;&emsp;&emsp;storageType|存储器类型|integer||
|&emsp;&emsp;&emsp;&emsp;size|文件大小|integer||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"items": [
			{
				"channelNo": 0,
				"startTime": "",
				"endTime": "",
				"warnBit1": 0,
				"warnBit2": 0,
				"mediaType": 0,
				"streamType": 0,
				"storageType": 0,
				"size": 0
			}
		]
	}
}
```


## 8801 摄像头立即拍摄命令


**接口地址**:`/device/8801`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "channelId": 0,
  "command": 0,
  "time": 0,
  "save": 0,
  "resolution": 0,
  "quality": 0,
  "brightness": 0,
  "contrast": 0,
  "saturation": 0,
  "chroma": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8801|T8801|body|true|T8801|T8801|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;channelId|通道ID(大于0)||true|integer(int32)||
|&emsp;&emsp;command|拍摄命令：0表示停止拍摄；65535表示录像；其它表示拍照张数||true|integer(int32)||
|&emsp;&emsp;time|拍照间隔/录像时间(秒) 0表示按最小间隔拍照或一直录像||true|integer(int32)||
|&emsp;&emsp;save|保存标志：1.保存 0.实时上传||true|integer(int32)||
|&emsp;&emsp;resolution|分辨率： 1.320*240 2.640*480 3.800*600 4.1024*768 5.176*144 [QCIF] 6.352*288 [CIF] 7.704*288 [HALF D1] 8.704*576 [D1]||true|integer(int32)||
|&emsp;&emsp;quality|图像/视频质量(1~10)：1.代表质量损失最小 10.表示压缩比最大||true|integer(int32)||
|&emsp;&emsp;brightness|亮度(0~255)||true|integer(int32)||
|&emsp;&emsp;contrast|对比度(0~127)||true|integer(int32)||
|&emsp;&emsp;saturation|饱和度(0~127)||true|integer(int32)||
|&emsp;&emsp;chroma|色度(0~255)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0805|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0805|T0805|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;result|结果：0.成功 1.失败 2.通道不支持|integer(int32)||
|&emsp;&emsp;id|多媒体ID列表|array|integer(int32)|
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"result": 0,
		"id": []
	}
}
```


## 8702 上报驾驶员身份信息请求


**接口地址**:`/device/8702`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0702|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0702|T0702|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;status|状态|integer(int32)||
|&emsp;&emsp;dateTime|时间|string||
|&emsp;&emsp;cardStatus|IC卡读取结果|integer(int32)||
|&emsp;&emsp;name|驾驶员姓名|string||
|&emsp;&emsp;licenseNo|从业资格证编码|string||
|&emsp;&emsp;institution|从业资格证发证机构名称|string||
|&emsp;&emsp;licenseValidPeriod|证件有效期|string||
|&emsp;&emsp;idCard|驾驶员身份证号|string||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"status": 0,
		"dateTime": "",
		"cardStatus": 0,
		"name": "",
		"licenseNo": "",
		"institution": "",
		"licenseValidPeriod": "",
		"idCard": ""
	}
}
```


## 8701 行驶记录仪参数下传命令


**接口地址**:`/device/8701`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "content": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8701|T8701|body|true|T8701|T8701|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|命令字： 130.设置车辆信息 131.设置记录仪初次安装日期 132.设置状态童配置信息 194.设置记录仪时间 195.设置记录仪脉冲系数 196.设置初始里程 197~207.预留||true|integer(int32)||
|&emsp;&emsp;content|数据块(参考GB/T 19056)||true|array|string(byte)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8700 行驶记录仪数据采集命令


**接口地址**:`/device/8700`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8608 查询区域或线路数据


**接口地址**:`/device/8608`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "id": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8608|T8608|body|true|T8608|T8608|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|查询类型：1.圆形 2.矩形 3.多边形 4.路线||true|integer(int32)||
|&emsp;&emsp;id|区域列表||true|array|integer(int32)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0608|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0608|T0608|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;type|查询类型|integer(int32)||
|&emsp;&emsp;bytes|查询返回的数据|array|string(byte)|
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"type": 0,
		"bytes": []
	}
}
```


## 8607 删除路线


**接口地址**:`/device/8607`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8601|T8601|body|true|T8601|T8601|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|区域列表||true|array|integer(int32)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8606 设置路线


**接口地址**:`/device/8606`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": 0,
  "attribute": 0,
  "startTime": "",
  "endTime": "",
  "items": [
    {
      "id": 0,
      "routeId": 0,
      "latitude": 0,
      "longitude": 0,
      "width": 0,
      "attribute": 0,
      "upperLimit": 0,
      "lowerLimit": 0,
      "maxSpeed": 0,
      "duration": 0,
      "nightMaxSpeed": 0
    }
  ],
  "name": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8606|T8606|body|true|T8606|T8606|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|路线ID||true|integer(int32)||
|&emsp;&emsp;attribute|路线属性||true|integer(int32)||
|&emsp;&emsp;startTime|起始时间(若区域属性0位为0则没有该字段)||true|string(date-time)||
|&emsp;&emsp;endTime|结束时间(若区域属性0位为0则没有该字段)||true|string(date-time)||
|&emsp;&emsp;items|拐点列表||true|array|Line|
|&emsp;&emsp;&emsp;&emsp;id|拐点ID||false|integer||
|&emsp;&emsp;&emsp;&emsp;routeId|路段ID||false|integer||
|&emsp;&emsp;&emsp;&emsp;latitude|纬度||false|integer||
|&emsp;&emsp;&emsp;&emsp;longitude|经度||false|integer||
|&emsp;&emsp;&emsp;&emsp;width|宽度(米)||false|integer||
|&emsp;&emsp;&emsp;&emsp;attribute|属性||false|integer||
|&emsp;&emsp;&emsp;&emsp;upperLimit|路段行驶过长阈值(秒,若区域属性0位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;lowerLimit|路段行驶不足阈值(秒,若区域属性0位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;maxSpeed|路段最高速度(公里每小时,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;duration|路段超速持续时间(秒,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;nightMaxSpeed|夜间最高速度(公里每小时,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;name|区域名称||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8605 删除多边形区域


**接口地址**:`/device/8605`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8601|T8601|body|true|T8601|T8601|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|区域列表||true|array|integer(int32)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8604 设置多边形区域


**接口地址**:`/device/8604`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": 0,
  "attribute": 0,
  "startTime": "",
  "endTime": "",
  "maxSpeed": 0,
  "duration": 0,
  "points": [
    {
      "latitude": 0,
      "longitude": 0
    }
  ],
  "nightMaxSpeed": 0,
  "name": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8604|T8604|body|true|T8604|T8604|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|区域ID||true|integer(int32)||
|&emsp;&emsp;attribute|区域属性||true|integer(int32)||
|&emsp;&emsp;startTime|起始时间(若区域属性0位为0则没有该字段)||true|string(date-time)||
|&emsp;&emsp;endTime|结束时间(若区域属性0位为0则没有该字段)||true|string(date-time)||
|&emsp;&emsp;maxSpeed|最高速度(公里每小时,若区域属性1位为0则没有该字段)||true|integer(int32)||
|&emsp;&emsp;duration|超速持续时间(秒,若区域属性1位为0则没有该字段)||true|integer(int32)||
|&emsp;&emsp;points|顶点项||true|array|Point|
|&emsp;&emsp;&emsp;&emsp;latitude|纬度||false|integer||
|&emsp;&emsp;&emsp;&emsp;longitude|经度||false|integer||
|&emsp;&emsp;nightMaxSpeed|夜间最高速度(公里每小时,若区域属性1位为0则没有该字段)||true|integer(int32)||
|&emsp;&emsp;name|区域名称||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8603 删除矩形区域


**接口地址**:`/device/8603`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8601|T8601|body|true|T8601|T8601|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|区域列表||true|array|integer(int32)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8602 设置矩形区域


**接口地址**:`/device/8602`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "action": 0,
  "items": [
    {
      "id": 0,
      "attribute": 0,
      "latitudeUL": 0,
      "longitudeUL": 0,
      "latitudeLR": 0,
      "longitudeLR": 0,
      "startTime": "",
      "endTime": "",
      "maxSpeed": 0,
      "duration": 0,
      "nightMaxSpeed": 0,
      "name": ""
    }
  ]
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8602|T8602|body|true|T8602|T8602|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;action|设置属性||true|integer(int32)||
|&emsp;&emsp;items|区域项||true|array|Rectangle|
|&emsp;&emsp;&emsp;&emsp;id|区域ID||false|integer||
|&emsp;&emsp;&emsp;&emsp;attribute|区域属性||false|integer||
|&emsp;&emsp;&emsp;&emsp;latitudeUL|左上点纬度||false|integer||
|&emsp;&emsp;&emsp;&emsp;longitudeUL|左上点经度||false|integer||
|&emsp;&emsp;&emsp;&emsp;latitudeLR|右下点纬度||false|integer||
|&emsp;&emsp;&emsp;&emsp;longitudeLR|右下点经度||false|integer||
|&emsp;&emsp;&emsp;&emsp;startTime|起始时间(若区域属性0位为0则没有该字段)||false|string||
|&emsp;&emsp;&emsp;&emsp;endTime|结束时间(若区域属性0位为0则没有该字段)||false|string||
|&emsp;&emsp;&emsp;&emsp;maxSpeed|最高速度(公里每小时,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;duration|超速持续时间(秒,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;nightMaxSpeed|夜间最高速度(公里每小时,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;name|区域名称||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8601 删除圆形区域


**接口地址**:`/device/8601`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8601|T8601|body|true|T8601|T8601|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|区域列表||true|array|integer(int32)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8600 设置圆形区域


**接口地址**:`/device/8600`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "action": 0,
  "items": [
    {
      "id": 0,
      "attribute": 0,
      "latitude": 0,
      "longitude": 0,
      "radius": 0,
      "startTime": "",
      "endTime": "",
      "maxSpeed": 0,
      "duration": 0,
      "nightMaxSpeed": 0,
      "name": ""
    }
  ]
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8600|T8600|body|true|T8600|T8600|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;action|设置属性||true|integer(int32)||
|&emsp;&emsp;items|区域项||true|array|Circle|
|&emsp;&emsp;&emsp;&emsp;id|区域ID||false|integer||
|&emsp;&emsp;&emsp;&emsp;attribute|区域属性||false|integer||
|&emsp;&emsp;&emsp;&emsp;latitude|中心点纬度||false|integer||
|&emsp;&emsp;&emsp;&emsp;longitude|中心点经度||false|integer||
|&emsp;&emsp;&emsp;&emsp;radius|半径(米)||false|integer||
|&emsp;&emsp;&emsp;&emsp;startTime|起始时间(若区域属性0位为0则没有该字段)||false|string||
|&emsp;&emsp;&emsp;&emsp;endTime|结束时间(若区域属性0位为0则没有该字段)||false|string||
|&emsp;&emsp;&emsp;&emsp;maxSpeed|最高速度(公里每小时,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;duration|超速持续时间(秒,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;nightMaxSpeed|夜间最高速度(公里每小时,若区域属性1位为0则没有该字段)||false|integer||
|&emsp;&emsp;&emsp;&emsp;name|区域名称||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8500 车辆控制


**接口地址**:`/device/8500`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "param": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8500|T8500|body|true|T8500|T8500|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|控制标志：[0]0.车门解锁 1.车门加锁 [1~7]保留||true|integer(int32)||
|&emsp;&emsp;param|控制标志：0.车门解锁 1.车门加锁||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0201_0500|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0201_0500|T0201_0500|
|&emsp;&emsp;warnBit|报警标志|integer(int32)||
|&emsp;&emsp;statusBit|状态|integer(int32)||
|&emsp;&emsp;latitude|纬度|integer(int32)||
|&emsp;&emsp;longitude|经度|integer(int32)||
|&emsp;&emsp;altitude|高程(米)|integer(int32)||
|&emsp;&emsp;speed|速度(1/10公里每小时)|integer(int32)||
|&emsp;&emsp;direction|方向|integer(int32)||
|&emsp;&emsp;deviceTime|时间(YYMMDDHHMMSS)|string(date-time)||
|&emsp;&emsp;lng||number(double)||
|&emsp;&emsp;lat||number(double)||
|&emsp;&emsp;speedKph||number(float)||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"warnBit": 0,
		"statusBit": 0,
		"latitude": 0,
		"longitude": 0,
		"altitude": 0,
		"speed": 0,
		"direction": 0,
		"deviceTime": "",
		"lng": 0,
		"lat": 0,
		"speedKph": 0,
		"responseSerialNo": 0
	},
	"success": true
}
```


## 8401 设置电话本


**接口地址**:`/device/8401`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "contacts": [
    {
      "sign": 0,
      "phone": "",
      "name": ""
    }
  ]
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8401|T8401|body|true|T8401|T8401|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|类型||true|integer(int32)||
|&emsp;&emsp;contacts|联系人项||true|array|Contact|
|&emsp;&emsp;&emsp;&emsp;sign|标志||false|integer||
|&emsp;&emsp;&emsp;&emsp;phone|电话号码||false|string||
|&emsp;&emsp;&emsp;&emsp;name|联系人||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8400 电话回拨


**接口地址**:`/device/8400`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "phoneNumber": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8400|T8400|body|true|T8400|T8400|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|类型：0.通话 1.监听||true|integer(int32)||
|&emsp;&emsp;phoneNumber|电话号码||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8304 信息服务


**接口地址**:`/device/8304`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "content": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8304|T8304|body|true|T8304|T8304|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|信息类型||true|integer(int32)||
|&emsp;&emsp;content|文本信息||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8303 信息点播菜单设置


**接口地址**:`/device/8303`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "infos": [
    {
      "type": 0,
      "name": ""
    }
  ]
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8303|T8303|body|true|T8303|T8303|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|设置类型||true|integer(int32)||
|&emsp;&emsp;infos|信息项||true|array|Info|
|&emsp;&emsp;&emsp;&emsp;type|信息类型||false|integer||
|&emsp;&emsp;&emsp;&emsp;name|信息名称||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8302 提问下发


**接口地址**:`/device/8302`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "sign": 0,
  "content": "",
  "options": [
    {
      "id": 0,
      "content": ""
    }
  ]
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8302|T8302|body|true|T8302|T8302|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;sign|标志： [0]紧急 [1]保留 [2]终端显示器显示 [3]终端 TTS 播读 [4]广告屏显示 [5]0.中心导航信息|1.CAN故障码信息 [6~7]保留||true|integer(int32)||
|&emsp;&emsp;content|问题||true|string||
|&emsp;&emsp;options|候选答案列表||true|array|Option|
|&emsp;&emsp;&emsp;&emsp;id|答案ID||false|integer||
|&emsp;&emsp;&emsp;&emsp;content|答案内容||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8301 事件设置


**接口地址**:`/device/8301`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "events": [
    {
      "id": 0,
      "content": ""
    }
  ]
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8301|T8301|body|true|T8301|T8301|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|设置类型：0.清空 1.更新(先清空,后追加) 2.追加 3.修改 4.指定删除||true|integer(int32)||
|&emsp;&emsp;events|事件项||true|array|Event|
|&emsp;&emsp;&emsp;&emsp;id|事件ID||false|integer||
|&emsp;&emsp;&emsp;&emsp;content|内容||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8300 文本信息下发


**接口地址**:`/device/8300`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "sign": 0,
  "type": 0,
  "content": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8300|T8300|body|true|T8300|T8300|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;sign|标志： [0]紧急 [1]保留 [2]终端显示器显示 [3]终端 TTS 播读 [4]广告屏显示 [5]0.中心导航信息|1.CAN故障码信息 [6~7]保留||true|integer(int32)||
|&emsp;&emsp;type|类型：1.通知 2.服务||true|integer(int32)||
|&emsp;&emsp;content|文本信息||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8204 服务器向终端发起链路检测请求


**接口地址**:`/device/8204`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8203 人工确认报警消息


**接口地址**:`/device/8203`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "responseSerialNo": 0,
  "type": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8203|T8203|body|true|T8203|T8203|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;responseSerialNo|消息流水号||true|integer(int32)||
|&emsp;&emsp;type|报警类型： [0]确认紧急报警 [1~2]保留 [3]确认危险预警 [4~19]保留 [20]确认进出区域报警 [21]确认进出路线报警 [22]确认路段行驶时间不足/过长报警 [23~26]保留 [27]确认车辆非法点火报警 [28]确认车辆非法位移报警 [29~31]保留||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8202 临时位置跟踪控制


**接口地址**:`/device/8202`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "interval": 0,
  "validityPeriod": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8202|T8202|body|true|T8202|T8202|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;interval|时间间隔(秒)||true|integer(int32)||
|&emsp;&emsp;validityPeriod|有效期(秒)||true|integer(int32)||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8201 位置信息查询


**接口地址**:`/device/8201`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0201_0500|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0201_0500|T0201_0500|
|&emsp;&emsp;warnBit|报警标志|integer(int32)||
|&emsp;&emsp;statusBit|状态|integer(int32)||
|&emsp;&emsp;latitude|纬度|integer(int32)||
|&emsp;&emsp;longitude|经度|integer(int32)||
|&emsp;&emsp;altitude|高程(米)|integer(int32)||
|&emsp;&emsp;speed|速度(1/10公里每小时)|integer(int32)||
|&emsp;&emsp;direction|方向|integer(int32)||
|&emsp;&emsp;deviceTime|时间(YYMMDDHHMMSS)|string(date-time)||
|&emsp;&emsp;lng||number(double)||
|&emsp;&emsp;lat||number(double)||
|&emsp;&emsp;speedKph||number(float)||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"warnBit": 0,
		"statusBit": 0,
		"latitude": 0,
		"longitude": 0,
		"altitude": 0,
		"speed": 0,
		"direction": 0,
		"deviceTime": "",
		"lng": 0,
		"lat": 0,
		"speedKph": 0,
		"responseSerialNo": 0
	},
	"success": true
}
```


## 8108 下发终端升级包


**接口地址**:`/device/8108`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "type": 0,
  "makerId": "",
  "packet": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8108|T8108|body|true|T8108|T8108|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;type|升级类型||true|integer(int32)||
|&emsp;&emsp;makerId|制造商ID,终端制造商编码||true|string||
|&emsp;&emsp;version|版本号||false|string||
|&emsp;&emsp;packet|数据包||true|array|string(byte)|
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8107 查询终端属性


**接口地址**:`/device/8107`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0107|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0107|T0107|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;deviceType|终端类型|integer(int32)||
|&emsp;&emsp;makerId|制造商ID,终端制造商编码|string||
|&emsp;&emsp;deviceModel|终端型号|string||
|&emsp;&emsp;deviceId|终端ID|string||
|&emsp;&emsp;iccid|终端SIM卡ICCID|string||
|&emsp;&emsp;hardwareVersion|硬件版本号|string||
|&emsp;&emsp;firmwareVersion|固件版本号|string||
|&emsp;&emsp;gnssAttribute|GNSS模块属性|integer(int32)||
|&emsp;&emsp;networkAttribute|通信模块属性|integer(int32)||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"deviceType": 0,
		"makerId": "",
		"deviceModel": "",
		"deviceId": "",
		"iccid": "",
		"hardwareVersion": "",
		"firmwareVersion": "",
		"gnssAttribute": 0,
		"networkAttribute": 0
	}
}
```


## 8106 查询指定终端参数


**接口地址**:`/device/8106`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "id": []
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8106|T8106|body|true|T8106|T8106|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;id|参数ID列表||true|array|integer(int32)|
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0104|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0104|T0104|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;total|应答参数个数|integer(int32)||
|&emsp;&emsp;parameters|参数项列表|object||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"total": 0,
		"parameters": {}
	}
}
```


## 8105 终端控制


**接口地址**:`/device/8105`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "command": 0,
  "parameter": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8105|T8105|body|true|T8105|T8105|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;command|命令字： 1.无线升级 2.控制终端连接指定服务器 3.终端关机 4.终端复位 5.终端恢复出厂设置 6.关闭数据通信 7.关闭所有无线通信||true|integer(int32)||
|&emsp;&emsp;parameter|命令参数||true|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```


## 8104 查询终端参数


**接口地址**:`/device/8104`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": ""
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|jTMessage|JTMessage|body|true|JTMessage|JTMessage|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0104|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0104|T0104|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;total|应答参数个数|integer(int32)||
|&emsp;&emsp;parameters|参数项列表|object||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"total": 0,
		"parameters": {}
	}
}
```


## 8103 设置终端参数


**接口地址**:`/device/8103`


**请求方式**:`POST`


**请求数据类型**:`application/x-www-form-urlencoded,application/json`


**响应数据类型**:`*/*`


**接口描述**:


**请求示例**:


```javascript
{
  "clientId": "",
  "total": 0,
  "parameters": {},
  "parametersInt": {},
  "parametersLong": {},
  "parametersStr": {},
  "paramImageIdentifyAlarm": {
    "overloadThreshold": "",
    "fatigueThreshold": ""
  },
  "paramVideoSpecialAlarm": {
    "storageThreshold": "",
    "duration": "",
    "startTime": ""
  },
  "paramChannels": {
    "audioVideoChannels": "",
    "audioChannels": "",
    "videoChannels": "",
    "channels": [
      {
        "channelId": "",
        "channelNo": "",
        "channelType": "",
        "hasPanTilt": true
      }
    ]
  },
  "paramSleepWake": {
    "mode": 0,
    "conditionType": 0,
    "dayOfWeek": 0,
    "timeFlag": 0,
    "wakeTime1": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "sleepTime1": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "wakeTime2": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "sleepTime2": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "wakeTime3": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "sleepTime3": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "wakeTime4": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    },
    "sleepTime4": {
      "hour": 0,
      "minute": 0,
      "second": 0,
      "nano": 0
    }
  },
  "paramVideo": {
    "realtimeEncode": "",
    "realtimeResolution": "",
    "realtimeFrameInterval": 0,
    "realtimeFrameRate": "",
    "realtimeBitRate": 0,
    "storageEncode": "",
    "storageResolution": "",
    "storageFrameInterval": 0,
    "storageFrameRate": "",
    "storageBitRate": 0,
    "odsConfig": 0,
    "audioEnable": ""
  },
  "paramVideoSingle": {
    "paramVideos": {
      "additionalProperties1": {
        "realtimeEncode": "",
        "realtimeResolution": "",
        "realtimeFrameInterval": 0,
        "realtimeFrameRate": "",
        "realtimeBitRate": 0,
        "storageEncode": "",
        "storageResolution": "",
        "storageFrameInterval": 0,
        "storageFrameRate": "",
        "storageBitRate": 0,
        "odsConfig": 0,
        "audioEnable": ""
      }
    }
  },
  "paramBSD": {
    "rearApproachAlarmTimeThreshold": "",
    "rearSideApproachAlarmTimeThreshold": ""
  },
  "paramTPMS": {
    "tireType": "",
    "pressureUnit": 0,
    "normalValue": 0,
    "imbalanceThreshold": 0,
    "lowLeakThreshold": 0,
    "lowPressureThreshold": 0,
    "highPressureThreshold": 0,
    "highTemperatureThreshold": 0,
    "voltageThreshold": 0,
    "reportInterval": 0
  },
  "paramDSM": {
    "p00": "",
    "p01": "",
    "p02": "",
    "p03": 0,
    "p05": 0,
    "p07": "",
    "p08": "",
    "p09": "",
    "p10": "",
    "p11": 0,
    "p15": 0,
    "p19": 0,
    "p21": 0,
    "p23": [],
    "p26": "",
    "p27": "",
    "p28": "",
    "p29": "",
    "p30": "",
    "p31": "",
    "p32": "",
    "p33": "",
    "p34": "",
    "p35": "",
    "p36": "",
    "p37": "",
    "p38": "",
    "p39": "",
    "p40": "",
    "p41": "",
    "p42": "",
    "p43": "",
    "p44": "",
    "p45": "",
    "p46": "",
    "p47": "",
    "p48": "",
    "p49": "",
    "p50": "",
    "p51": "",
    "p52": "",
    "p53": "",
    "p54": "",
    "p55": "",
    "p56": "",
    "p57": "",
    "p58": "",
    "p59": "",
    "p60": "",
    "p61": "",
    "p62": "",
    "p63": "",
    "p64": ""
  },
  "paramADAS": {
    "p00": "",
    "p01": "",
    "p02": "",
    "p03": 0,
    "p05": 0,
    "p07": "",
    "p08": "",
    "p09": "",
    "p10": "",
    "p11": 0,
    "p15": 0,
    "p19": "",
    "p20": "",
    "p21": "",
    "p22": "",
    "p23": "",
    "p24": "",
    "p25": "",
    "p26": "",
    "p27": "",
    "p28": "",
    "p29": "",
    "p30": "",
    "p31": "",
    "p32": "",
    "p33": "",
    "p34": "",
    "p35": "",
    "p36": "",
    "p37": "",
    "p38": "",
    "p39": "",
    "p40": "",
    "p41": "",
    "p42": "",
    "p43": "",
    "p44": "",
    "p45": "",
    "p46": "",
    "p47": "",
    "p48": "",
    "p49": "",
    "p50": "",
    "p51": "",
    "p52": "",
    "p53": "",
    "p54": "",
    "p55": "",
    "p56": ""
  },
  "bodyLength": 0,
  "encryption": 0
}
```


**请求参数**:


| 参数名称 | 参数说明 | 请求类型    | 是否必须 | 数据类型 | schema |
| -------- | -------- | ----- | -------- | -------- | ------ |
|t8103|T8103|body|true|T8103|T8103|
|&emsp;&emsp;messageId|消息ID||false|integer(int32)||
|&emsp;&emsp;properties|消息体属性||false|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号||false|integer(int32)||
|&emsp;&emsp;clientId|终端手机号||true|string||
|&emsp;&emsp;serialNo|流水号||false|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数||false|integer(int32)||
|&emsp;&emsp;packageNo|包序号||false|integer(int32)||
|&emsp;&emsp;verified|||false|boolean||
|&emsp;&emsp;total|参数总数||true|integer(int32)||
|&emsp;&emsp;parameters|参数项列表||true|object||
|&emsp;&emsp;parametersInt|数值型参数列表(BYTE、WORD)||false|object||
|&emsp;&emsp;parametersLong|数值型参数列表(DWORD、QWORD)||false|object||
|&emsp;&emsp;parametersStr|字符型参数列表||false|object||
|&emsp;&emsp;paramImageIdentifyAlarm|图像分析报警参数设置(1078)||false|ParamImageIdentifyAlarm|ParamImageIdentifyAlarm|
|&emsp;&emsp;&emsp;&emsp;overloadThreshold|车辆核载人数,客运车辆核定载客人数,视频分析结果超过时产生报警||false|string||
|&emsp;&emsp;&emsp;&emsp;fatigueThreshold|疲劳程度阈值,视频分析疲劳驾驶报警阈值,超过时产生报警||false|string||
|&emsp;&emsp;paramVideoSpecialAlarm|特殊报警录像参数设置(1078)||false|ParamVideoSpecialAlarm|ParamVideoSpecialAlarm|
|&emsp;&emsp;&emsp;&emsp;storageThreshold|特殊报警录像存储阈值(占用主存储器存储阈值百分比,取值1~99.默认值为20)||false|string||
|&emsp;&emsp;&emsp;&emsp;duration|特殊报警录像持续时间,特殊报警录像的最长持续时间(分钟),默认值为5||false|string||
|&emsp;&emsp;&emsp;&emsp;startTime|特殊报警标识起始时间,特殊报警发生前进行标记的录像时间(分钟),默认值为1||false|string||
|&emsp;&emsp;paramChannels|音视频通道列表设置(1078)||false|ParamChannels|ParamChannels|
|&emsp;&emsp;&emsp;&emsp;audioVideoChannels|音视频通道总数||false|string||
|&emsp;&emsp;&emsp;&emsp;audioChannels|音频通道总数||false|string||
|&emsp;&emsp;&emsp;&emsp;videoChannels|视频通道总数||false|string||
|&emsp;&emsp;&emsp;&emsp;channels|音视频通道对照表||false|array|ChannelInfo|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;channelId|物理通道号(从1开始)||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;channelNo|逻辑通道号(按照JT/T 1076-2016 中的表2)||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;channelType|通道类型：0.音视频 1.音频 2.视频||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hasPanTilt|是否连接云台(类型为0和2时,此字段有效)：0.未连接 1.连接||false|boolean||
|&emsp;&emsp;paramSleepWake|终端休眠唤醒模式设置数据格式(1078)||false|ParamSleepWake|ParamSleepWake|
|&emsp;&emsp;&emsp;&emsp;mode|休眠唤醒模式：[0]条件唤醒 [1]定时唤醒 [2]手动唤醒||false|integer||
|&emsp;&emsp;&emsp;&emsp;conditionType|唤醒条件类型：[0]紧急报警 [1]碰撞侧翻报警 [2]车辆开门,休眠唤醒模式中[0]为1时此字段有效,否则置0||false|integer||
|&emsp;&emsp;&emsp;&emsp;dayOfWeek|周定时唤醒日设置：[0]周一 [1]周二 [2]周三 [3]周四 [4]周五 [5]周六 [6]周日||false|integer||
|&emsp;&emsp;&emsp;&emsp;timeFlag|日定时唤醒启用标志：[0]启用时间段1 [1]启用时间段2 [2]启用时间段3 [3]启用时间段4)||false|integer||
|&emsp;&emsp;&emsp;&emsp;wakeTime1|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;sleepTime1|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;wakeTime2|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;sleepTime2|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;wakeTime3|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;sleepTime3|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;wakeTime4|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;&emsp;&emsp;sleepTime4|||false|LocalTime|LocalTime|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;hour|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;minute|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;second|||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;nano|||false|integer||
|&emsp;&emsp;paramVideo|||false|ParamVideo|ParamVideo|
|&emsp;&emsp;&emsp;&emsp;realtimeEncode|实时流编码模式：0.CBR(固定码率) 1.VBR(可变码率) 2.ABR(平均码率) 100~ 127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;realtimeResolution|实时流分辨率：0.QCIF 1.CIF 2.WCIF 3.D1 4.WD1 5.720P 6.1080P 100~127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;realtimeFrameInterval|实时流关键帧间隔(1~1000帧)||false|integer||
|&emsp;&emsp;&emsp;&emsp;realtimeFrameRate|实时流目标帧率(1~120帧)||false|string||
|&emsp;&emsp;&emsp;&emsp;realtimeBitRate|实时流目标码率(kbps)||false|integer||
|&emsp;&emsp;&emsp;&emsp;storageEncode|存储流编码模式：0.CBR(固定码率) 1.VBR(可变码率) 2.ABR(平均码率) 100~ 127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;storageResolution|存储流分辨率：0.QCIF 1.CIF 2.WCIF 3.D1 4.WD1 5.720P 6.1080P 100~127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;storageFrameInterval|存储流关键帧间隔(1~1000帧)||false|integer||
|&emsp;&emsp;&emsp;&emsp;storageFrameRate|存储流目标帧率(1~120帧)||false|string||
|&emsp;&emsp;&emsp;&emsp;storageBitRate|存储流目标码率(kbps)||false|integer||
|&emsp;&emsp;&emsp;&emsp;odsConfig|OSD字幕叠加设置(按位,0.表示不叠加 1.表示叠加)： [0]日期和时间 [1]车牌号码 [2]逻辑通道号 [3]经纬度 [4]行驶记录速度 [5]卫星定位速度 [6]连续驾驶时间 [7~l0]保留 [11~l5]自定义||false|integer||
|&emsp;&emsp;&emsp;&emsp;audioEnable|是否启用音频输出：0.不启用 1.启用||false|string||
|&emsp;&emsp;paramVideoSingle|单独视频通道参数设置(1078)||false|ParamVideoSingle|ParamVideoSingle|
|&emsp;&emsp;&emsp;&emsp;paramVideos|单独通道视频参数设置列表||false|ParamVideo|ParamVideo|
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;realtimeEncode|实时流编码模式：0.CBR(固定码率) 1.VBR(可变码率) 2.ABR(平均码率) 100~ 127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;realtimeResolution|实时流分辨率：0.QCIF 1.CIF 2.WCIF 3.D1 4.WD1 5.720P 6.1080P 100~127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;realtimeFrameInterval|实时流关键帧间隔(1~1000帧)||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;realtimeFrameRate|实时流目标帧率(1~120帧)||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;realtimeBitRate|实时流目标码率(kbps)||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageEncode|存储流编码模式：0.CBR(固定码率) 1.VBR(可变码率) 2.ABR(平均码率) 100~ 127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageResolution|存储流分辨率：0.QCIF 1.CIF 2.WCIF 3.D1 4.WD1 5.720P 6.1080P 100~127.自定义||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageFrameInterval|存储流关键帧间隔(1~1000帧)||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageFrameRate|存储流目标帧率(1~120帧)||false|string||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;storageBitRate|存储流目标码率(kbps)||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;odsConfig|OSD字幕叠加设置(按位,0.表示不叠加 1.表示叠加)： [0]日期和时间 [1]车牌号码 [2]逻辑通道号 [3]经纬度 [4]行驶记录速度 [5]卫星定位速度 [6]连续驾驶时间 [7~l0]保留 [11~l5]自定义||false|integer||
|&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;audioEnable|是否启用音频输出：0.不启用 1.启用||false|string||
|&emsp;&emsp;paramBSD|盲区监测系统参数(苏标)||false|ParamBSD|ParamBSD|
|&emsp;&emsp;&emsp;&emsp;rearApproachAlarmTimeThreshold|后方接近报警时间阈值||false|string||
|&emsp;&emsp;&emsp;&emsp;rearSideApproachAlarmTimeThreshold|侧后方接近报警时间阈值||false|string||
|&emsp;&emsp;paramTPMS|胎压监测系统参数(苏标)||false|ParamTPMS|ParamTPMS|
|&emsp;&emsp;&emsp;&emsp;tireType|轮胎规格型号(例：195/65R1591V,12个字符,默认值'900R20')||false|string||
|&emsp;&emsp;&emsp;&emsp;pressureUnit|胎压单位：0.kg/cm2 1.bar 2.Kpa 3.PSI(默认值3)||false|integer||
|&emsp;&emsp;&emsp;&emsp;normalValue|正常胎压值(同胎压单位,默认值140)||false|integer||
|&emsp;&emsp;&emsp;&emsp;imbalanceThreshold|胎压不平衡报警阈值(百分比0~100,达到冷态气压值,默认值20)||false|integer||
|&emsp;&emsp;&emsp;&emsp;lowLeakThreshold|慢漏气报警阈值(百分比0~100,达到冷态气压值,默认值5)||false|integer||
|&emsp;&emsp;&emsp;&emsp;lowPressureThreshold|低压报警阈值(同胎压单位,默认值110)||false|integer||
|&emsp;&emsp;&emsp;&emsp;highPressureThreshold|高压报警阈值(同胎压单位,默认值189)||false|integer||
|&emsp;&emsp;&emsp;&emsp;highTemperatureThreshold|高温报警阈值(摄氏度,默认值80)||false|integer||
|&emsp;&emsp;&emsp;&emsp;voltageThreshold|电压报警阈值(百分比0~100,默认值10)||false|integer||
|&emsp;&emsp;&emsp;&emsp;reportInterval|定时上报时间间隔(秒,取值0~3600,默认值60,0表示不上报)||false|integer||
|&emsp;&emsp;&emsp;&emsp;reserved|保留项||false|array|string|
|&emsp;&emsp;paramDSM|驾驶员状态监测系统参数(苏标)||false|ParamDSM|ParamDSM|
|&emsp;&emsp;&emsp;&emsp;p00|报警判断速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p01|报警音量 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p02|主动拍照策略 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p03|主动定时拍照时间间隔 WORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p05|主动定距拍照距离间隔 WORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p07|单次主动拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p08|单次主动拍照时间间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p09|拍照分辨率 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p10|视频录制分辨率 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p11|报警使能 DWORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p15|事件使能 DWORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p19|吸烟报警判断时间间隔 WORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p21|接打电话报警判断时间间隔 WORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p23|预留字段 BYTE[3]||false|array|string|
|&emsp;&emsp;&emsp;&emsp;p26|疲劳驾驶报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p27|疲劳驾驶报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p28|疲劳驾驶报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p29|疲劳驾驶报警拍照间隔时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p30|接打电话报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p31|接打电话报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p32|接打电话报警拍驾驶员面部特征照片张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p33|接打电话报警拍驾驶员面部特征照片间隔时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p34|抽烟报警分级车速阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p35|抽烟报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p36|抽烟报警拍驾驶员面部特征照片张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p37|抽烟报警拍驾驶员面部特征照片间隔时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p38|分神驾驶报警分级车速阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p39|分神驾驶报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p40|分神驾驶报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p41|分神驾驶报警拍照间隔时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p42|驾驶行为异常分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p43|驾驶行为异常视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p44|驾驶行为异常抓拍照片张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p45|驾驶行为异常拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p46|驾驶员身份识别触发 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p47|摄像机遮挡报警分级速度阈值(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p48|不系安全带报警分级速度阈值(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p49|不系安全带报警前后视频录制时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p50|不系安全带报警抓拍照片张数(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p51|不系安全带报警抓拍照片间隔时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p52|红外墨镜阻断失效报警分级速度阈值(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p53|红外墨镜阻断失效报警前后视频录制时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p54|红外墨镜阻断失效报警抓拍照片张数(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p55|红外墨镜阻断失效报警抓拍照片间隔时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p56|双脱把报警分级速度阈值(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p57|双脱把报警前后视频录制时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p58|双脱把报警抓拍照片张数(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p59|双脱把报警抓拍照片间隔时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p60|玩手机报警分级速度阈值(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p61|玩手机报警前后视频录制时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p62|玩手机报警抓拍照片张数(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p63|玩手机报警拍抓拍，照片间隔时间(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p64|保留字段(粤标)||false|string||
|&emsp;&emsp;paramADAS|高级驾驶辅助系统参数(苏标)||false|ParamADAS|ParamADAS|
|&emsp;&emsp;&emsp;&emsp;p00|报警判断速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p01|报警提示音量 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p02|主动拍照策略 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p03|主动定时拍照时间间隔 WORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p05|主动定距拍照距离间隔 WORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p07|单次主动拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p08|单次主动拍照时间间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p09|拍照分辨率 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p10|视频录制分辨率 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p11|报警使能 DWORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p15|事件使能 DWORD||false|integer||
|&emsp;&emsp;&emsp;&emsp;p19|预留字段 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p20|障碍物报警距离阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p21|障碍物报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p22|障碍物报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p23|障碍物报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p24|障碍物报警拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p25|频繁变道报警判断时间段 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p26|频繁变道报警判断次数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p27|频繁变道报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p28|频繁变道报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p29|频繁变道报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p30|频繁变道报警拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p31|车道偏离报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p32|车道偏离报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p33|车道偏离报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p34|车道偏离报警拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p35|前向碰撞报警时间阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p36|前向碰撞报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p37|前向碰撞报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p38|前向碰撞报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p39|前向碰撞报警拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p40|行人碰撞报警时间阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p41|行人碰撞报警使能速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p42|行人碰撞报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p43|行人碰撞报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p44|行人碰撞报警拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p45|车距监控报警距离阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p46|车距监控报警分级速度阈值 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p47|车距过近报警前后视频录制时间 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p48|车距过近报警拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p49|车距过近报警拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p50|道路标志识别拍照张数 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p51|道路标志识别拍照间隔 BYTE||false|string||
|&emsp;&emsp;&emsp;&emsp;p52|实线变道报警分级速度阈值 BYTE(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p53|实线变道报警前后视频录制时间 BYTE(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p54|实线变道报警拍照张数 BYTE(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p55|实线变道报警拍照间隔 BYTE(粤标)||false|string||
|&emsp;&emsp;&emsp;&emsp;p56|车厢过道行人检测报警分级速度阈值 BYTE(粤标)||false|string||
|&emsp;&emsp;version|||false|boolean||
|&emsp;&emsp;bodyLength|||false|integer(int32)||
|&emsp;&emsp;subpackage|||false|boolean||
|&emsp;&emsp;encryption|||false|integer(int32)||
|&emsp;&emsp;reserved|||false|boolean||


**响应状态**:


| 状态码 | 说明 | schema |
| -------- | -------- | ----- | 
|200|OK|APIResultT0001|


**响应参数**:


| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|code|响应码(成功：200；客户端错误：400-499；服务端错误：500-599)|integer(int32)|integer(int32)|
|msg|响应消息|string||
|detailMsg|响应消息详情|string||
|data||T0001|T0001|
|&emsp;&emsp;messageId|消息ID|integer(int32)||
|&emsp;&emsp;properties|消息体属性|integer(int32)||
|&emsp;&emsp;protocolVersion|协议版本号|integer(int32)||
|&emsp;&emsp;clientId|终端手机号|string||
|&emsp;&emsp;serialNo|流水号|integer(int32)||
|&emsp;&emsp;packageTotal|消息包总数|integer(int32)||
|&emsp;&emsp;packageNo|包序号|integer(int32)||
|&emsp;&emsp;verified||boolean||
|&emsp;&emsp;responseSerialNo|应答流水号|integer(int32)||
|&emsp;&emsp;responseMessageId|应答ID|integer(int32)||
|&emsp;&emsp;resultCode|结果：0.成功 1.失败 2.消息有误 3.不支持 4.报警处理确认|integer(int32)||
|&emsp;&emsp;success||boolean||
|&emsp;&emsp;version||boolean||
|&emsp;&emsp;bodyLength||integer(int32)||
|&emsp;&emsp;subpackage||boolean||
|&emsp;&emsp;encryption||integer(int32)||
|&emsp;&emsp;reserved||boolean||
|success||boolean||


**响应示例**:
```javascript
{
	"code": 0,
	"msg": "",
	"detailMsg": "",
	"data": {
		"clientId": "",
		"responseSerialNo": 0,
		"responseMessageId": 0,
		"resultCode": 0
	}
}
```