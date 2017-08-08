---
title: UsrCloud.dll 用户手册

language_tabs:

  - csharp
  - pascal
  - cpp

toc_footers:
  - 版本  :1.0
  - 日期  :2017-7-26
  - <a href='http://cloud.usr.cn'>有人透传云</a>

search: true
---

# 快速上手

### 函数调用流程

1. **准备工作**
    USR_Init       : 初始化 , 创建实例。
    USR\_OnXXXXXX  : 注册回调函数 , 在 USR\_Init 后执行。
    USR_Connect    : 连接服务器。
2. **通讯**
    USR_Subscribe  : 订阅。告诉服务器, 想接收哪些设备的数据。
    USR_Publish    : 推送。向设备发送数据。
3. **收尾工作**
    USR_DisConnect : 断开与服务器的连接。
    USR_Release    : 释放资源。程序结束前 , 务必调用此方法。

### 回调函数用法
  
&emsp;以 [  USR_OnConnAck  ](#Define_USR_OnConnAck) 举例 , 其他回调函数类似

&emsp;| 说明 | 举例
---- | ---- | ----
  1  | 用户需要按照回调函数定义 , 自定义回调函数 | 按照 [ TUSR_ConnAckEvent 定义 ](#Define_TUSR_ConnAckEvent) , 写回调函数<br>ConnAck\_CBF( long ReturnCode , LPCWSTR Description )<br>（参见右侧示例代码或 Demo ） 
  2  | 用dll提供的USR\_OnXXXXXX方法 , 将回调函数地址传给dll | 执行  USR_OnConnAck(ConnAck\_CBF)
  3  | 当事件发生时 , 会触发用户写的回调函数 | 执行 USR\_Connect 连接服务器 , 会收到服务器的反馈 , 触发 ConnAck\_CBF , 通过 ReturnCode 参数 , 可判断是否连接成功。
  
# 接口说明

## --------- 初始化和释放 ---------

## <aside>USR_GetVer 获取dll版本</aside>

> USR_GetVer 获取dll版本 声明:

```pascal
function USR_GetVer: LongInt; stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_GetVer",
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_GetVer();
```
```cpp

```
```vb

```
> 调用

```pascal
Writeln('dll版本号:' + IntToStr(USR_GetVer));
```
```csharp
Log("dll版本号: " + USR_GetVer().ToString());
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`long USR_GetVer();`**

返回值| 描述
---- | ----
long| DLL版本号

## <aside>USR_Init 初始化接口</aside>

> USR_Init 初始化接口 声明:

```pascal
function USR_Init(Host: PWideChar; Port: Word; Ver: LongInt): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Init", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_Init(string host, ushort port, int vertion);
```
```cpp

```
```vb

```
> 调用,一般在窗口创建时调用 

```pascal
if USR_Init(PWideChar('clouddata.usr.cn'), 1883, 1) then
begin
   { 初始化成功, 一般在这里设置回调函数 }
end;
```
```csharp
if (USR_Init("clouddata.usr.cn", 1883, 1))
{
   /* 初始化成功, 一般在这里设置回调函数 */
}
```
```cpp

```
```vb

```
<br>

**函数原型:<br><br>`boolean USR_Init(LPCWSTR Host, unsigned short Port, long Ver);`**

参数| 描述
---- | ----
Host|[in] 透传云服务器地址 clouddata.usr.cn
Port|[in] 端口 1883
Ver |[in] 指定你想使用的版本号,必须 <= 函数**USR_GetVer**查询到的最高版本号,目前只能填 1。

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

## <aside>USR_Release 释放接口</aside>

> USR_Release 释放接口 声明:

```pascal
function USR_Release(): Boolean; stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Release", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_Release();
```
```cpp

```
```vb

```
> 调用,一般在窗口释放时调用

```pascal
if USR_Release() then
begin
  { 释放成功 }
end;
```
```csharp
if(USR_Release())
{
    Log("释放成功");
}
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`boolean USR_Release();`**

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false


## --------- 连接和断开 ---------

<span id = "Define_USR_OnConnAck"></span>
## <aside>USR_OnConnAck 设置 连接响应 回调函数</aside>

> USR_OnConnAck 设置 连接响应 回调函数 声明:

```pascal
TUSR_ConnAckEvent = procedure(ReturnCode: LongInt;
  Description: PWideChar); stdcall;
```
```pascal
function USR_OnConnAck(OnConnAct: TUSR_ConnAckEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
public delegate void TUSR_ConnAckEvent(int returnCode, IntPtr description);
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnConnAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnConnAck(TUSR_ConnAckEvent OnConnAck);
```
```cpp

```
```vb

```
> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于判断是否连接成功 }
procedure ConnAck_CBF(ReturnCode: Integer; Description: PWideChar);
var
  vs                : string;
begin
  case ReturnCode of
    $00:
      begin
        { 连接成功  }
      end;
  end;
  vs := Description;
  Writeln('ReturnCode:' + IntToStr(ReturnCode) + ' ;' + vs);
end;
```
```pascal
{ 注册回调函数 }
USR_OnConnAck(ConnAck_CBF);
```
```csharp
/* 自定义回调函数,用于判断是否连接成功 */
private void ConnAck_CBF(int returnCode, IntPtr description)
{
    Log("【连接回调】");
    Log("returnCode: " + returnCode.ToString() + "  " + 
      Marshal.PtrToStringAuto(description));
    if (returnCode==0)
    {
        Log("连接成功");
    }
    else
    {
        Log("连接失败");
    }
}
```
```csharp
TUSR_ConnAckEvent FConnAck_CBF;
FConnAck_CBF = new TUSR_ConnAckEvent(ConnAck_CBF);
/* 注册回调函数 */
USR_OnConnAck(FConnAck_CBF);
```
```cpp

```
```vb

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

## <aside>USR_Connect 连接</aside>

> USR_Connect 连接 声明:

```pascal
function USR_Connect(Username, Password: PWideChar): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Connect", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_Connect(string Username, string Password);
```
```cpp

```
```vb

```
> 调用

```pascal
if USR_Connect(PWideChar('sdktest'),PWideChar('sdktest')) then
begin
  { 连接已发起,请在USR_OnConnAck设置的回调函数中判断连接结果 }
end;
```
```csharp
if (USR_Connect("sdktest", "sdktest"))
{
    Log("连接已发起");
}
```
```cpp

```
```vb

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

## <aside>USR_DisConnect 断开连接</aside>

> USR_DisConnect 断开连接 声明:

```pascal
function USR_DisConnect(): Boolean; stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_DisConnect", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_DisConnect();
```
```cpp

```
```vb

```
> 调用

```pascal
if USR_DisConnect() then
begin
  { 断开成功  }
end;
```
```csharp
if (USR_DisConnect())
{
    Log("已断开");
}
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`boolean USR_DisConnect();`**

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

## --------- 订阅消息 ---------
想要接收哪个设备发来的消息,就要订阅哪个设备的消息。

## <aside>USR_OnSubAck 设置 订阅响应 回调函数</aside>

> USR_OnSubAck 设置 订阅响应 回调函数 声明:

```pascal
TUSR_SubAckEvent = procedure(MessageID: LongInt;
  DevId, ReturnCode: PWideChar); stdcall;
```
```pascal
function USR_OnSubAck(OnSubAck: TUSR_SubAckEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
public delegate void TUSR_SubAckEvent(int messageID, 
    IntPtr devId, IntPtr returnCode);
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnSubAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnSubAck(TUSR_SubAckEvent OnSubAck);
```
```cpp

```
```vb

```
> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于判断是否订阅成功 }
procedure SubAck_CBF(MessageID: Integer; DevId, ReturnCode: PWideChar);
var
  vsHint      : string;
begin
  vsHint := Format(
    '订阅响应:' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID(或用户名):%s' + Chr(13) + Chr(10) +
    '结果分别是:%s',
    [MessageID,
     WideCharToString(DevId),
     WideCharToString(ReturnCode)]);
  Writeln(vsHint);
end;
```
```pascal
{ 注册回调函数 }
USR_OnSubAck(SubAck_CBF);
```
```csharp
/* 自定义回调函数,用于判断订阅结果 */
private void SubAck_CBF(int messageID, IntPtr devId, IntPtr returnCode)
{
    string sDevId= Marshal.PtrToStringAuto(devId);
    string sReturnCode = Marshal.PtrToStringAuto(returnCode); 
    string[] devIdArray = sDevId.Split(',');
    string[] retCodeArray = sReturnCode.Split(',');
    int len = devIdArray.Length;

    Log("【订阅回调】");
    Log("MsgId:" + messageID.ToString());
    for (int i = 0; i < len; ++i)
    {
        Log("设备：" + devIdArray[i] + "  订阅结果：" + retCodeArray[i]);
    }
}
```
```csharp
TUSR_SubAckEvent FSubAck_CBF;
FSubAck_CBF = new TUSR_SubAckEvent(SubAck_CBF);
/* 注册回调函数 */
USR_OnSubAck(FSubAck_CBF);
```
```cpp

```
```vb

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

## <aside>USR_Subscribe 订阅消息</aside>

> USR_Subscribe 订阅消息 声明:

```pascal
function USR_Subscribe(DevId: PWideChar): LongInt; 
stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Subscribe", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_Subscribe(string devId);
```
```cpp

```
```vb

```
> 调用

```pascal
if USR_Subscribe(
    PWideChar('00000000000000000001,00000000000000000002')
    ) > -1 then
begin
  { 已发起订阅两个设备00000000000000000001,00000000000000000002
    请在USR_OnSubAck设置的回调函数中判断订阅结果  }
end;
  
if USR_Subscribe(PWideChar('')) > -1 then
begin
  { 已发起订阅账户下所有设备 
    请在USR_OnSubAck设置的回调函数中判断订阅结果  }
end;
```
```csharp
int iMsgId = USR_Subscribe("00000000000000000001");
if(iMsgId > -1)
{
    Log("订阅设备00000000000000000001 已发起");  
}
```
```csharp
int iMsgId = USR_Subscribe("");
if(iMsgId > -1)
{
    Log("订阅所有设备 已发起");  
}
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`long USR_Subscribe(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要订阅哪个设备发来的消息。<br>**如果要订阅多个,请用逗号隔开;<br>如果要订阅帐号下所有的设备的消息,请传入空。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnSubAck设置的回调函数来判断。

## <aside>USR_OnUnSubAck 设置 取消订阅响应回调函数</aside>

> USR_OnUnSubAck 设置 取消订阅响应回调函数 声明:

```pascal
TUSR_UnSubAckEvent = procedure(MessageID: LongInt; 
  DevId: PWideChar); stdcall;
```
```pascal
function USR_OnUnSubAck(OnUnSubAck: TUSR_UnSubAckEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
public delegate void TUSR_UnSubAckEvent(int messageID, IntPtr devId);
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnUnSubAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnUnSubAck(TUSR_UnSubAckEvent OnUnSubAck);
```
```cpp

```
```vb

```
> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数  }
procedure UnSubAck_CBF(MessageID: Integer; DevId: PWideChar);
var
  vsHint            : string;
begin
  vsHint := Format('取消订阅响应:' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID(或用户名):%s',
    [MessageID, WideCharToString(DevId)]);
  Writeln(vsHint);
end;
```
```pascal
{ 注册回调函数 }
USR_OnUnSubAck(UnSubAck_CBF);
```
```csharp
/* 自定义回调函数 */
private void UnSubAck_CBF(int messageID, IntPtr devId)
{
    string sDevId = Marshal.PtrToStringAuto(devId);
    Log("【取消订阅回调】");
    Log("MsgId:" + messageID.ToString());
    Log("设备：" + sDevId);
}
```
```csharp
TUSR_UnSubAckEvent FUnSubAck_CBF;
FUnSubAck_CBF = new TUSR_UnSubAckEvent(UnSubAck_CBF);
/* 注册回调函数 */
USR_OnUnSubAck(FUnSubAck_CBF);
```
```cpp

```
```vb

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

## <aside>USR_UnSubscribe 取消订阅</aside>

> USR_UnSubscribe 取消订阅 声明:

```pascal
function USR_UnSubscribe(DevId: PWideChar): LongInt; 
stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_UnSubscribe", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_UnSubscribe(string devId);
```
```cpp

```
```vb

```
> 调用

```pascal
if USR_UnSubscribe(
    PWideChar('00000000000000000001,00000000000000000002')
    ) > -1 then
begin
  { 已发起取消订阅两个设备00000000000000000001,00000000000000000002
    请在USR_OnUnSubAck设置的回调函数中判断结果 }
end;
  
if USR_UnSubscribe(PWideChar('')) > -1 then
begin
  { 已发起取消订阅账户下所有设备 
    请在USR_OnUnSubAck设置的回调函数中判断结果 }
end;
```
```csharp
int iMsgId = USR_UnSubscribe("00000000000000000001");
if(iMsgId > -1)
{
    Log("取消订阅设备00000000000000000001 已发起");  
}
```
```csharp
int iMsgId = USR_UnSubscribe("");
if(iMsgId > -1)
{
    Log("取消订阅所有设备 已发起");  
}
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`long USR_UnSubscribe(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要取消订阅哪个设备发来的消息。<br>**如果要取消订阅多个,请用逗号隔开;<br>如果要取消订阅帐号下所有的设备的消息,请传入空。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnUnSubAck设置的回调函数来判断。


## --------- 推送消息 ---------

## <aside>USR_OnPubAck 设置 推送响应 回调函数</aside>

> USR_OnPubAck 设置 推送响应 回调函数 声明:

```pascal
TUSR_PubAckEvent = procedure(MessageID: LongInt); stdcall;
```
```pascal
function USR_OnPubAck(OnPubAck: TUSR_PubAckEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
public delegate void TUSR_PubAckEvent(int MessageID);
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnPubAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnPubAck(TUSR_PubAckEvent OnPubAck);
```
```cpp

```
```vb

```
> 调用

```pascal
{ 自定义回调函数,用于判断是否推送成功  }
procedure PubAck_CBF(MessageID: Integer);
begin
  Writeln('收到推送确认,MessageID: ' + IntToStr(MessageID));
end;
```
```pascal
{ 注册回调函数 }
USR_OnPubAck(PubAck_CBF);
```
```csharp
/* 自定义回调函数,用于判断是否推送成功 */
protected void PubAck_CBF(int messageID)
{
    Log("【推送回调】");
    Log("MsgId:" + messageID.ToString());
}
```
```csharp
TUSR_PubAckEvent FPubAck_CBF;
FPubAck_CBF = new TUSR_PubAckEvent(PubAck_CBF);
/* 注册回调函数 */
USR_OnPubAck(FPubAck_CBF);
```
```cpp

```
```vb

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
### **TUSR_PubAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_PubAckEvent )(
                long MessageID
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。用于判断推送的哪条消息得到服务器的响应了。

## <aside>USR_Publish 推送数据</aside>

> USR_Publish 推送数据 声明:

```pascal
function USR_Publish(DevId: PWideChar; pData: PByte; DataLen: Integer)
  : LongInt; stdcall; external 'UsrCloud.dll';
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Publish", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_Publish(string DevId, 
    byte[] pData, int DataLen);
```
```cpp

```
```vb

```
> 调用

```pascal
var
  viMsgId           : Integer;
  vsData            : string;
begin
  vsData := 'abcd';
  viMsgId := USR_Publish(
      PWideChar('00000000000000000001'),
      @vsData[1], Length(vsData)
      );
  if viMsgId > -1 then
    Writeln('消息已推送 MsgId:' + IntToStr(viMsgId));
end;
```
```csharp
byte[] byteArray = new byte[] { 0x01, 0x02, 0x03 };
int iMsgId = USR_Publish("00000000000000000001", 
                         byteArray, 
                         byteArray.Length);
if (iMsgId > -1) 
{
    Log("消息已推送 MsgId:" + iMsgId.ToString());
}
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`long USR_Publish(LPCWSTR DevId, void *pData,long DataLen);`**

参数| 描述
---- | ----
DevId  | [in] 设备ID,指定要把数据发给哪个设备。**只能填一个**
pData | [in] 数据
DataLen | [in] 数据长度

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

## --------- 接收消息 ---------

## <aside>USR_OnRcv 设置 收到数据 回调函数</aside>

> USR_OnRcv 设置 收到数据 回调函数 声明:

```pascal
TUSR_RcvEvent = procedure(MessageID: LongInt; DevId: PWideChar;
  pData: PByte; DataLen: Integer); stdcall;
```
```pascal
function USR_OnRcv(OnRcv: TUSR_RcvEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```
```csharp
public delegate void TUSR_OnRcvEvent(int MessageID, 
    IntPtr DevId, IntPtr pData, int DataLen);
```
```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnRcv", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnRcv(TUSR_OnRcvEvent OnRcvEvent);
```
```cpp

```
```vb

```
> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于处理接收的数据  }
procedure Rcv_CBF(MessageID: LongInt; DevId: PWideChar;
  pData: PByte; DataLen: Integer);
var
  vsHint, vsHexData : string;
begin
  vsHexData := Pbuf2HexStr(PByteArray(pData), DataLen);
  vsHint := Format(
    '收到数据' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID:%s' + Chr(13) + Chr(10) +
    '内容(HEX):%s',
    [MessageID, 
     WideCharToString(DevId), 
     vsHexData]);

  Writeln(vsHint);
end;
```
```pascal
{ 注册回调函数 }
USR_OnRcv(Rcv_CBF);
```
```csharp
/* 自定义回调函数,用于处理接收的数据 */
private void Rcv_CBF(int messageID, IntPtr devId, IntPtr pData, int DataLen)
{
    string sDevId = Marshal.PtrToStringAuto(devId);
    byte[] byteArr = new byte[DataLen];
    Marshal.Copy(pData, byteArr, 0, DataLen);
    string sHex = BitConverter.ToString(byteArr).Replace("-", " ");
    Log("【接收回调】");
    Log("设备ID   : " + sDevId);
    Log("MsgId    : " + messageID.ToString());
    Log("接收数据(Hex): " + sHex);
}
```
```csharp
TUSR_OnRcvEvent FRcv_CBF;
FRcv_CBF = new TUSR_OnRcvEvent(Rcv_CBF);
/* 注册回调函数 */
USR_OnRcv(FRcv_CBF);
```
```cpp

```
```vb

```

<br>

**函数原型:<br><br>`boolean USR_OnRcv(TUSR_RcvEvent OnRcv);`**

参数|描述
----|----
OnRcv |[in] 接收数据 回调函数   [TUSR_RcvEvent 定义](#Define_TUSR_RcvEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_RcvEvent"></span>
### **TUSR_RcvEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_RcvEvent )(
                long MessageID,
                LPCWSTR DevId,
                void *pData,
                long DataLen
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。
DevId|[out] 设备ID,消息来源
pData|[out] 数据起始地址
DataLen|[out] 数据长度

# 更新历史

版本 | 日期 | 更新内容 | 更新人
---- | ---- | ---- | ----
1.0 | 2017-7-26 | 初版 | 张振鸣