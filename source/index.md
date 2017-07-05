---
title: UsrCloud.dll 用户手册

language_tabs:

  - pascal
  - csharp
  - cpp
  - vb

toc_footers:
  - 版本  :1.0
  - 日期  :2017-5-4
  - <a href='http://cloud.usr.cn'>有人透传云</a>

search: true
---

<aside class="success">**目录**</aside>

<!-- TOC -->

- [快速上手](#快速上手)
- [接口说明](#接口说明)
    - [--------- 初始化和释放 ---------](#----------初始化和释放----------)
    - [<aside>USR_GetVer 获取dll版本</aside>](#asideusr_getver-获取dll版本aside)
    - [<aside>USR_Init 初始化接口</aside>](#asideusr_init-初始化接口aside)
    - [<aside>USR_Release 释放接口</aside>](#asideusr_release-释放接口aside)
    - [--------- 连接和断开 ---------](#----------连接和断开----------)
    - [<aside>USR_OnConnAck 设置 连接响应 回调函数</aside>](#asideusr_onconnack-设置-连接响应-回调函数aside)
        - [**TUSR_ConnAckEvent 定义**](#tusr_connackevent-定义)
    - [<aside>USR_Connect 连接</aside>](#asideusr_connect-连接aside)
    - [<aside>USR_DisConnect 断开连接</aside>](#asideusr_disconnect-断开连接aside)
    - [--------- 订阅消息 ---------](#----------订阅消息----------)
    - [<aside>USR_OnSubAck 设置 订阅响应 回调函数</aside>](#asideusr_onsuback-设置-订阅响应-回调函数aside)
        - [**TUSR_SubAckEvent 定义**](#tusr_subackevent-定义)
    - [<aside>USR_Subscribe 订阅消息</aside>](#asideusr_subscribe-订阅消息aside)
    - [<aside>USR_OnUnSubAck 设置 取消订阅响应回调函数</aside>](#asideusr_onunsuback-设置-取消订阅响应回调函数aside)
        - [**TUSR_UnSubAckEvent 定义**](#tusr_unsubackevent-定义)
    - [<aside>USR_UnSubscribe 取消订阅</aside>](#asideusr_unsubscribe-取消订阅aside)
    - [--------- 推送消息 ---------](#----------推送消息----------)
    - [<aside>USR_OnPubAck 设置 推送响应 回调函数</aside>](#asideusr_onpuback-设置-推送响应-回调函数aside)
        - [**TUSR_PubAckEvent 定义**](#tusr_pubackevent-定义)
    - [<aside>USR_Publish_Stream 推送透传数据</aside>](#asideusr_publish_stream-推送透传数据aside)
    - [<aside>USR_Publish_ATCmd 推送AT指令</aside>](#asideusr_publish_atcmd-推送at指令aside)
    - [--------- 接收消息 ---------](#----------接收消息----------)
    - [<aside>USR_OnRcvStream 设置 接收到透传数据消息 回调函数</aside>](#asideusr_onrcvstream-设置-接收到透传数据消息-回调函数aside)
        - [**TUSR_OnRcvStreamEvent 定义**](#tusr_onrcvstreamevent-定义)
    - [<aside>USR_OnRcvATCmd 设置 接收到AT指令消息 回调函数</aside>](#asideusr_onrcvatcmd-设置-接收到at指令消息-回调函数aside)
        - [**TUSR_OnRcvATCmdEvent 定义**](#tusr_onrcvatcmdevent-定义)
    - [<aside>USR_OnRcvDevStatus 设置 接收到设备状态变化 回调函数</aside>](#asideusr_onrcvdevstatus-设置-接收到设备状态变化-回调函数aside)
        - [**TUSR_RcvDevStatusEvent 定义**](#tusr_rcvdevstatusevent-定义)
    - [<aside>USR_OnRcvErrorCode 设置 接收到错误消息 回调函数</aside>](#asideusr_onrcverrorcode-设置-接收到错误消息-回调函数aside)
        - [**TUSR__RcvErrorCodeEvent 定义**](#tusr__rcverrorcodeevent-定义)
- [更新历史](#更新历史)

<!-- /TOC -->

<aside class="success">**正文**</aside>

<a id="markdown-快速上手" name="快速上手"></a>
# 快速上手

todo 展示最基本的流程
  
<a id="markdown-接口说明" name="接口说明"></a>
# 接口说明

<a id="markdown-----------初始化和释放----------" name="----------初始化和释放----------"></a>
## --------- 初始化和释放 ---------

<a id="markdown-asideusr_getver-获取dll版本aside" name="asideusr_getver-获取dll版本aside"></a>
## <aside>USR_GetVer 获取dll版本</aside>

> USR_GetVer 获取dll版本 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`long USR_GetVer();`**

返回值| 描述
---- | ----
long| DLL版本号

<a id="markdown-asideusr_init-初始化接口aside" name="asideusr_init-初始化接口aside"></a>
## <aside>USR_Init 初始化接口</aside>

> USR_Init 初始化接口 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_Init(LPCWSTR Host, unsigned short Port, long Ver);`**

参数| 描述
---- | ----
Host|[in] 透传云服务器地址 cloud.usr.cn
Port|[in] 端口 1883
Ver |[in] 指定你想使用的版本号,必须 <= 函数USR_GetVer查询到的最高版本号,目前只能填 1。

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<a id="markdown-asideusr_release-释放接口aside" name="asideusr_release-释放接口aside"></a>
## <aside>USR_Release 释放接口</aside>

> USR_Release 释放接口 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_Release();`**

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false


<a id="markdown-----------连接和断开----------" name="----------连接和断开----------"></a>
## --------- 连接和断开 ---------

<a id="markdown-asideusr_onconnack-设置-连接响应-回调函数aside" name="asideusr_onconnack-设置-连接响应-回调函数aside"></a>
## <aside>USR_OnConnAck 设置 连接响应 回调函数</aside>

> USR_OnConnAck 设置 连接响应 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnConnAck(TUSR_ConnAckEvent OnConnAct);`**

参数|描述
----|----
OnConnAct |[in] 连接响应 回调函数   [TUSR_ConnAckEvent 定义](#Define_TUSR_ConnAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_ConnAckEvent"></span>
<a id="markdown-tusr_connackevent-定义" name="tusr_connackevent-定义"></a>
### **TUSR_ConnAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_ConnAckEvent)(
                long ReturnCode,
                LPCWSTR Description
                );`**

参数|描述
----|----
ReturnCode |[in] 返回码
Description|[in] 返回码代表的含义

返回值|描述
----|----
boolean|成功返回 true ,失败返回 false

可能的返回码如下:

返回码|返回码代表的含义
----|----
0x00 |连接已接受
0x01 |连接已拒绝，不支持的协议版本
0x02 |连接已拒绝，不合格的客户端标识符
0x03 |连接已拒绝，服务端不可用
0x04 |连接已拒绝，无效的用户名或密码
0x05 |连接已拒绝，未授权

<a id="markdown-asideusr_connect-连接aside" name="asideusr_connect-连接aside"></a>
## <aside>USR_Connect 连接</aside>

> USR_Connect 连接 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_Connect(LPCWSTR Username, LPCWSTR Password);`**

参数| 描述
---- | ----
Username|[in] 用户名
Password|[in] 密码

返回值| 描述
---- | ----
boolean| 返回true说明两个问题:<br>(1).成功和服务器建立了TCP连接<br>(2).成功将用户名、密码等验证信息发给了服务器。<br><br>最终有没有被服务器所接受,要通过USR_OnConnAck设置的回调函数来判断。

<a id="markdown-asideusr_disconnect-断开连接aside" name="asideusr_disconnect-断开连接aside"></a>
## <aside>USR_DisConnect 断开连接</aside>

> USR_DisConnect 断开连接 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_DisConnect();`**

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<a id="markdown-----------订阅消息----------" name="----------订阅消息----------"></a>
## --------- 订阅消息 ---------
想要接收哪个设备发来的消息,就要订阅哪个设备的消息。

<a id="markdown-asideusr_onsuback-设置-订阅响应-回调函数aside" name="asideusr_onsuback-设置-订阅响应-回调函数aside"></a>
## <aside>USR_OnSubAck 设置 订阅响应 回调函数</aside>

> USR_OnSubAck 设置 订阅响应 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnSubAck(TUSR_SubAckEvent OnSubAck);`**

参数|描述
----|----
OnSubAck |[in] 订阅响应 回调函数   [TUSR_SubAckEvent 定义](#Define_TUSR_SubAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_SubAckEvent"></span>
<a id="markdown-tusr_subackevent-定义" name="tusr_subackevent-定义"></a>
### **TUSR_SubAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_SubAckEvent)(
                long MessageID,
                LPCWSTR DevId,
                LPCWSTR ReturnCode
                );`**

参数|描述
----|----
MessageID |[out] 消息ID,一般用不到
DevId |[out] 设备ID,多个用逗号隔开
ReturnCode|[out] 返回码,表示订阅结果,多个用逗号隔开,和DevId依次对应。

ReturnCode可能的值有:

返回码| 返回码代表的含义
---- | ----
0   | 最大QoS 0
1   | 成功 – 最大QoS 1
2   | 成功 – 最大 QoS 2
128 | Failure 失败

<a id="markdown-asideusr_subscribe-订阅消息aside" name="asideusr_subscribe-订阅消息aside"></a>
## <aside>USR_Subscribe 订阅消息</aside>

> USR_Subscribe 订阅消息 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`long USR_Subscribe(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要订阅哪个设备发来的消息。<br>**如果要订阅多个,请用逗号隔开;<br>如果要订阅帐号下所有的设备的消息,请传入空。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnSubAck设置的回调函数来判断。

<a id="markdown-asideusr_onunsuback-设置-取消订阅响应回调函数aside" name="asideusr_onunsuback-设置-取消订阅响应回调函数aside"></a>
## <aside>USR_OnUnSubAck 设置 取消订阅响应回调函数</aside>

> USR_OnUnSubAck 设置 取消订阅响应回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnUnSubAck(TUSR_UnSubAckEvent OnUnSubAck);`**

参数|描述
----|----
OnUnSubAck |[in] 取消订阅 响应  回调函数   [TUSR_UnSubAckEvent 定义](#Define_TUSR_UnSubAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_UnSubAckEvent"></span>
<a id="markdown-tusr_unsubackevent-定义" name="tusr_unsubackevent-定义"></a>
### **TUSR_UnSubAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_UnSubAckEvent )(
                long MessageID, 
                LPCWSTR DevId
                );`**

参数|描述
----|----
MessageID |[out] 消息ID,一般用不到
DevId |[out] 设备ID,多个用逗号隔开

<a id="markdown-asideusr_unsubscribe-取消订阅aside" name="asideusr_unsubscribe-取消订阅aside"></a>
## <aside>USR_UnSubscribe 取消订阅</aside>

> USR_UnSubscribe 取消订阅 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`long USR_UnSubscribe(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要取消订阅哪个设备发来的消息。<br>**如果要取消订阅多个,请用逗号隔开;<br>如果要取消订阅帐号下所有的设备的消息,请传入空。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnUnSubAck设置的回调函数来判断。


<a id="markdown-----------推送消息----------" name="----------推送消息----------"></a>
## --------- 推送消息 ---------

<a id="markdown-asideusr_onpuback-设置-推送响应-回调函数aside" name="asideusr_onpuback-设置-推送响应-回调函数aside"></a>
## <aside>USR_OnPubAck 设置 推送响应 回调函数</aside>

> USR_OnPubAck 设置 推送响应 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnPubAck(TUSR_PubAckEvent OnPubAck);`**

参数|描述
----|----
OnPubAck |[in] 推送响应 回调函数   [TUSR_PubAckEvent 定义](#Define_TUSR_TUSR_PubAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_TUSR_PubAckEvent"></span>
<a id="markdown-tusr_pubackevent-定义" name="tusr_pubackevent-定义"></a>
### **TUSR_PubAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_PubAckEvent )(
                long MessageID
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。用于判断推送的哪条消息得到服务器的响应了。

<a id="markdown-asideusr_publish_stream-推送透传数据aside" name="asideusr_publish_stream-推送透传数据aside"></a>
## <aside>USR_Publish_Stream 推送透传数据</aside>

> USR_Publish_Stream 推送透传数据 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`long USR_Publish_Stream(LPCWSTR DevId, void *APbyte,long Len);`**

参数| 描述
---- | ----
DevId  | [in] 设备ID,指定要把数据发给哪个设备。**只能填一个**
APbyte | [in] 数据
Len    | [in] 数据长度

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

<a id="markdown-asideusr_publish_atcmd-推送at指令aside" name="asideusr_publish_atcmd-推送at指令aside"></a>
## <aside>USR_Publish_ATCmd 推送AT指令</aside>

> USR_Publish_ATCmd 推送AT指令 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`long USR_Publish_ATCmd(LPCWSTR DevId, LPCWSTR ATCmd);`**

参数| 描述
---- | ----
DevId  | [in] 设备ID,指定要把数据发给哪个设备。**只能填一个**
ATCmd  | [in] AT指令内容

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

<a id="markdown-----------接收消息----------" name="----------接收消息----------"></a>
## --------- 接收消息 ---------

<a id="markdown-asideusr_onrcvstream-设置-接收到透传数据消息-回调函数aside" name="asideusr_onrcvstream-设置-接收到透传数据消息-回调函数aside"></a>
## <aside>USR_OnRcvStream 设置 接收到透传数据消息 回调函数</aside>

> USR_OnRcvStream 设置 接收到透传数据消息 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnRcvStream(TUSR_OnRcvStreamEvent OnRcvStream);`**

参数|描述
----|----
OnRcvStream |[in] 接收到透传数据消息 回调函数   [TUSR_OnRcvStreamEvent 定义](#Define_TUSR_OnRcvStreamEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_OnRcvStreamEvent"></span>
<a id="markdown-tusr_onrcvstreamevent-定义" name="tusr_onrcvstreamevent-定义"></a>
### **TUSR_OnRcvStreamEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_OnRcvStreamEvent )(
                long MessageID,
                LPCWSTR DevId,
                void *APbyte,
                long Len
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。
DevId|[out] 设备ID,消息来源
APbyte|[out] 数据起始地址
Len|[out] 数据长度


<a id="markdown-asideusr_onrcvatcmd-设置-接收到at指令消息-回调函数aside" name="asideusr_onrcvatcmd-设置-接收到at指令消息-回调函数aside"></a>
## <aside>USR_OnRcvATCmd 设置 接收到AT指令消息 回调函数</aside>

> USR_OnRcvATCmd 设置 接收到AT指令消息 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnRcvATCmd(TUSR_RcvATCmdEvent OnRcvATCmd);`**

参数|描述
----|----
OnRcvATCmd|[in] 接收到AT指令消息 回调函数   [TUSR_OnRcvATCmdEvent 定义](#Define_TUSR_OnRcvATCmdEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_OnRcvATCmdEvent"></span>
<a id="markdown-tusr_onrcvatcmdevent-定义" name="tusr_onrcvatcmdevent-定义"></a>
### **TUSR_OnRcvATCmdEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_OnRcvATCmdEvent )(
                long MessageID,
                LPCWSTR DevId,
                LPCWSTR ATCmd
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。
DevId|[out] 设备ID,消息来源
ATCmd|[out] AT指令内容

<a id="markdown-asideusr_onrcvdevstatus-设置-接收到设备状态变化-回调函数aside" name="asideusr_onrcvdevstatus-设置-接收到设备状态变化-回调函数aside"></a>
## <aside>USR_OnRcvDevStatus 设置 接收到设备状态变化 回调函数</aside>

> USR_OnRcvDevStatus 设置 接收到设备状态变化 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnRcvDevStatus(TUSR_RcvDevStatusEvent OnRcvDevStatus);`**

参数|描述
----|----
OnRcvDevStatus|[in] 接收到设备状态变化 回调函数   [TUSR_RcvDevStatusEvent 定义](#Define_TUSR_RcvDevStatusEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_RcvDevStatusEvent"></span>
<a id="markdown-tusr_rcvdevstatusevent-定义" name="tusr_rcvdevstatusevent-定义"></a>
### **TUSR_RcvDevStatusEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_RcvDevStatusEvent)(
                long MessageID,
                LPCWSTR DevId,
                long Status
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。
DevId|[out] 设备ID,消息来源
Status|[out] 设备状态<br>0x01 设备上线<br>0x00 设备离线


<a id="markdown-asideusr_onrcverrorcode-设置-接收到错误消息-回调函数aside" name="asideusr_onrcverrorcode-设置-接收到错误消息-回调函数aside"></a>
## <aside>USR_OnRcvErrorCode 设置 接收到错误消息 回调函数</aside>

> USR_OnRcvErrorCode 设置 接收到错误消息 回调函数 用法示例:

```pascal
[ pascal ]
```
```csharp
[ C# ]
```
```cpp
[ C++ ]
```
```vb
[ VB ]
```

<br>

**函数原型:<br><br>`boolean USR_OnRcvErrorCode(TUSR__RcvErrorCodeEvent OnRcvErrorCode);`**

参数|描述
----|----
OnRcvErrorCode|[in] 接收到错误消息 回调函数   [TUSR__RcvErrorCodeEvent 定义](#Define_TUSR__RcvErrorCodeEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR__RcvErrorCodeEvent"></span>
<a id="markdown-tusr__rcverrorcodeevent-定义" name="tusr__rcverrorcodeevent-定义"></a>
### **TUSR__RcvErrorCodeEvent 定义**

<br>

**`typedef void(__stdcall *TUSR__RcvErrorCodeEvent )(
                long MessageID,
                byte ErrorCode,
                LPCWSTR Description
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。
ErrorCode|[out] 错误码
Description|[out] 错误描述

错误码及其含义如下:

参数|描述
----|----
0x00| 密码错误
0x01| 帐号错误
0x02| 账户欠费
0x03| 账号下不存在该设备
其他| 未知命令,请更新dll

<a id="markdown-更新历史" name="更新历史"></a>
# 更新历史

版本 | 日期 | 更新内容 | 更新人
---- | ---- | ---- | ----
1.0 | 2017-5-27 | 初版 | 张振鸣
