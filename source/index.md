---
title: UsrCloud.dll 用户手册

language_tabs:

  - csharp
  - pascal
  - cpp

toc_footers:
  - 版本  :2.1.0
  - 日期  :2017-12-7
  - <a href='http://cloud.usr.cn'>有人透传云</a>
  - <style>.fwb{font-weight:bold;}</style>
  - <style>a{ text-decoration:none}</style>

search: true
---

# 快速上手

### 函数调用流程

1. **准备工作**
    + USR_Init       : 初始化 , 创建实例。
    + USR_OnXXXXXX  : 注册回调函数 , 在 USR_Init 后执行。
    + USR_Connect    : 连接服务器。
2. **通讯**
    + USR_Subscribe  : 订阅。告诉服务器, 想接收哪些设备的数据。
    + USR_Publish    : 推送。向设备发送数据。
3. **收尾工作**
    + USR_DisConnect : 断开与服务器的连接。
    + USR_Release    : 释放资源。程序结束前 , 务必调用此方法。

### 回调函数用法
  
&emsp;以 [  USR_OnConnAck  ](#Define_USR_OnConnAck) 举例 , 其他回调函数类似

&emsp;| 说明 | 举例
---- | ---- | ----
  1  | 用户需要按照回调函数定义 , 自定义回调函数 | 按照 [ TUSR_ConnAckEvent 定义 ](#Define_TUSR_ConnAckEvent) , 写回调函数<br>ConnAck_CBF( long ReturnCode , LPCWSTR Description )<br>（参见右侧示例代码或 Demo ） 
  2  | 用dll提供的USR_OnXXXXXX方法 , 将回调函数地址传给dll | 执行  USR_OnConnAck(ConnAck_CBF)
  3  | 当事件发生时 , 会触发用户写的回调函数 | 执行 USR_Connect 连接服务器 , 会收到服务器的反馈 , 触发 ConnAck_CBF , 通过 ReturnCode 参数 , 可判断是否连接成功。

# 接口说明

## 函数汇总

<table border="1" frame=vsides bordercolor="#8FBCD4" align="center">    <tr>        <td colspan="2" align="center"><b>功能说明</b></td><td align="center"><b>云交换机</b>            <br>操作原始数据</td><td align="center"><b>云组态</b>            <br>操作解析后的数据</td>    </tr><tr>        <td rowspan="3" align="center">初始化和释放</td><td>获取版本号</td><td colspan="2" align="center"><a href="#USR-GetVer-获取dll版本" class="fwb">USR_GetVer</a></td>    </tr><tr>        <td>初始化</td><td colspan="2" align="center"><a href="#USR-Init-初始化接口" class="fwb">USR_Init</a></td>    </tr><tr>        <td>释放</td><td colspan="2" align="center"><a href="#USR-Release-释放接口" class="fwb">USR_Release</a></td>    </tr><tr>        <td rowspan="3" align="center">连接和断开</td><td>连接回调</td><td colspan="2" align="center"><a href="#USR-OnConnAck-设置-连接响应回调函数" class="fwb">USR_OnConnAck</a></td>    </tr><tr>        <td>连接</td><td colspan="2" align="center"><a href="#USR-Connect-连接" class="fwb">USR_Connect</a></td>    </tr><tr>        <td>断开</td><td colspan="2" align="center"><a href="#USR-DisConnect-断开连接" class="fwb">USR_DisConnect</a></td>    </tr><tr>        <td rowspan="6" align="center">订阅和取消订阅</td><td>订阅回调</td><td colspan="2" align="center"><a href="#USR-OnSubscribeAck-设置-订阅响应回调函数" class="fwb">USR_OnSubscribeAck</a></td>    </tr><tr>        <td>订阅设备数据</td><td align="center"><a href="#USR-SubscribeDevRaw-订阅单个设备原始数据流" class="fwb">USR_SubscribeDevRaw</a></td><td align="center"><a href="#USR-SubscribeDevParsed-订阅单个设备解析后的数据" class="fwb">USR_SubscribeDevParsed</a></td>    </tr><tr>        <td>订阅账户下所有设备数据</td><td align="center"><a href="#USR-SubscribeUserRaw-订阅账户下所有设备原始数据流" class="fwb">USR_SubscribeUserRaw</a></td><td align="center"><a href="#USR-SubscribeUserParsed-订阅账户下所有设备解析后的数据" class="fwb">USR_SubscribeUserParsed</a></td>    </tr><tr>        <td>取消订阅回调</td><td colspan="2" align="center"><a href="#USR-OnUnSubscribeAck-设置-取消订阅响应回调函数" class="fwb">USR_OnUnSubscribeAck</a></td>    </tr><tr>        <td>取消订阅设备数据</td><td align="center"><a href="#USR-UnSubscribeDevRaw-取消订阅单个设备原始数据流" class="fwb">USR_UnSubscribeDevRaw</a></td><td align="center"><a href="#USR-UnSubscribeDevParsed-取消订阅单个设备解析后的数据" class="fwb">USR_UnSubscribeDevParsed</a></td>    </tr><tr>        <td>取消订阅账户下所有设备数据</td><td align="center"><a href="#USR-UnSubscribeUserRaw-取消订阅账户下所有设备原始数据流" class="fwb">USR_UnSubscribeUserRaw</a></td><td align="center"><a href="#USR-UnSubscribeUserParsed-取消订阅账户下所有设备解析后的数据" class="fwb">USR_UnSubscribeUserParsed</a></td>    </tr><tr>        <td rowspan="2" align="center">推送数据</td><td>推送回调</td><td colspan="2" align="center"><a href="#USR-OnPubAck-设置-推送响应回调函数" class="fwb">USR_OnPubAck</a></td>    </tr><tr>        <td>推送数据</td><td align="center"><a href="#USR-PublishRawToDev-向单台设备推送原始数据流" class="fwb">USR_PublishRawToDev</a>            <br align="center"><a href="#USR-PublishRawToUser-向账户下所有设备推送原始数据流" class="fwb">USR_PublishRawToUser</a></td><td align="center"><a href="#USR-PublishParsedSetDataPoint-设置单台设备数据点值" class="fwb">USR_PublishParsedSetDataPoint</a>            <br align="center"><a href="#USR-PublishParsedQueryDataPoint-查询单台设备数据点值" class="fwb">USR_PublishParsedQueryDataPoint</a></td>    </tr><tr>        <td colspan="1" align="center">接收数据</td><td>接收数据</td><td align="center"><a href="#USR-OnRcvRawFromDev-设置-接收设备原始数据流回调函数" class="fwb">USR_OnRcvRawFromDev</a></td><td align="center"><a href="#USR-OnRcvParsedDataPointPush-设置-接收设备数据点值推送回调函数" class="fwb">USR_OnRcvParsedDataPointPush</a>            <br align="center"><a href="#USR-OnRcvParsedDevStatusPush-设置-接收设备上下线推送回调函数" class="fwb">USR_OnRcvParsedDevStatusPush</a>            <br align="center"><a href="#USR-OnRcvParsedDevAlarmPush-设置-接收设备报警推送回调函数" class="fwb">USR_OnRcvParsedDevAlarmPush</a>            <br align="center"><a href="#USR-OnRcvParsedOptionResponseReturn-设置-接收设备数据点操作应答回调函数" class="fwb">USR_OnRcvParsedOptionResponseReturn</a></td>    </tr></table>

## --------- 初始化和释放 ---------

### <aside>USR_GetVer 获取dll版本</aside>

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
typedef long(_stdcall *FN_USR_GetVer)();
```

> 调用

```pascal
Writeln('dll版本号:' + IntToStr(USR_GetVer));
```

```csharp
Log("dll版本号: " + USR_GetVer().ToString());
```

```cpp
/*载入DLL*/
HINSTANCE hUsrCloud;
hUsrCloud = LoadLibraryA("UsrCloud.dll");
/*获取函数地址*/
FN_USR_GetVer USR_GetVer;
USR_GetVer = (FN_USR_GetVer)GetProcAddress(hUsrCloud, "USR_GetVer");
long iVer = USR_GetVer();
CString sVer;
sVer.Format(_T("DLL版本号: %d\n"), iVer);
/*dll用完了之后, 释放掉*/
FreeLibrary(hUsrCloud);
```

<br>

**函数原型:<br><br>`long USR_GetVer();`**

返回值| 描述
---- | ----
long| DLL版本号

### <aside>USR_Init 初始化接口</aside>

> USR_Init 初始化接口 声明:

```pascal
function USR_Init(
    Host: PWideChar; Port: Word; Ver: LongInt): Boolean;
    stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Init", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_Init(
    string host, ushort port, int vertion);
```

```cpp
typedef boolean(_stdcall *FN_USR_Init)(
    LPCWSTR Host, unsigned short Port, long Ver);
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
/*GetProc*/
FN_USR_Init USR_Init;
USR_Init = (FN_USR_Init)GetProcAddress(hUsrCloud, "USR_Init");
/*use*/
CString sHost, sPort;
m_Edit_Host.GetWindowTextW(sHost);
m_Edit_Port.GetWindowTextW(sPort);
LPCWSTR Host = (LPCWSTR)sHost;
unsigned short Port = _ttoi(sPort);
CString str;
boolean b = USR_Init(Host, Port, 2);
if (b) {
    str.Format(_T("初始化成功\n"));
    /* 初始化成功, 一般在这里设置回调函数 */
};
```

<br>

**函数原型:<br><br>`boolean USR_Init(LPCWSTR Host, unsigned short Port, long Ver);`**

参数| 描述
---- | ----
Host|[in] 透传云服务器地址 clouddata.usr.cn
Port|[in] 端口 1883
Ver |[in] 指定你想使用的DLL版本号,目前可用的值有1和2。<br>为解决日后可能出现的兼容性问题而生, 暂时用不到。

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

### <aside>USR_Release 释放接口</aside>

> USR_Release 释放接口 声明:

```pascal
function USR_Release(): Boolean; 
stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Release", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_Release();
```

```cpp
typedef boolean(_stdcall *FN_USR_Release)();
```

> 调用,一般在窗口释放时调用

```pascal
if USR_Release then
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
FN_USR_Release USR_Release;
USR_Release = (FN_USR_Release)GetProcAddress(
    hUsrCloud, "USR_Release");

boolean b = USR_Release();
CString str;
if (b) {
    str.Format(_T("释放成功\n"));
};
```

<br>

**函数原型:<br><br>`boolean USR_Release();`**

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

## --------- 连接和断开 ---------

<span id = "Define_USR_OnConnAck"></span>
### <aside>USR_OnConnAck 设置 连接响应回调函数</aside>

> USR_OnConnAck 设置 连接响应回调函数 声明:

```pascal
TUSR_ConnAckEvent = procedure(
    ReturnCode: LongInt; Description: PWideChar); stdcall;
```

```pascal
function USR_OnConnAck(OnConnAct: TUSR_ConnAckEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_ConnAckEvent(
    int returnCode, IntPtr description);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnConnAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnConnAck(
    TUSR_ConnAckEvent OnConnAck);
```

```cpp
typedef void(_stdcall *TUSR_ConnAckEvent)(
    long ReturnCode, LPCWSTR Description);
typedef boolean(_stdcall *FN_USR_OnConnAck)(
    TUSR_ConnAckEvent OnConnAct);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于判断是否连接成功 }
procedure ConnAck_CBF(
    ReturnCode: Integer; Description: PWideChar);
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
static void _stdcall ConnAck_CBF(
    long ReturnCode, LPCWSTR Description);
    
/* 自定义回调函数,用于判断是否连接成功 */
void CUsrCloudDllDemoDlg::ConnAck_CBF(
    long ReturnCode, LPCWSTR Description)
{
	CString str;
	str.Format(_T("【连接事件】\n返回码：%d  描述： %s\n"), 
        ReturnCode, Description);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}
/* 注册回调函数 */
FN_USR_OnConnAck USR_OnConnAck;
USR_OnConnAck = (FN_USR_OnConnAck)GetProcAddress(
    hUsrCloud, "USR_OnConnAck");
USR_OnConnAck(ConnAck_CBF);
```

<br>

**函数原型:<br><br>`boolean USR_OnConnAck(TUSR_ConnAckEvent OnConnAct);`**

参数|描述
----|----
OnConnAct |[in] 连接响应回调函数   [TUSR_ConnAckEvent 定义](#Define_TUSR_ConnAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_ConnAckEvent"></span>
**TUSR_ConnAckEvent 定义**

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

### <aside>USR_Connect 连接</aside>

> USR_Connect 连接 声明:

```pascal
function USR_Connect(
    Username, Password: PWideChar): Boolean; 
    stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_Connect", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_Connect(
    string Username, string Password);
```

```cpp
typedef boolean(_stdcall *FN_USR_Connect)(
    LPCWSTR Username, LPCWSTR Password);
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
FN_USR_Connect USR_Connect;
USR_Connect = (FN_USR_Connect)GetProcAddress(hUsrCloud, "USR_Connect");

CString sUserName, sPassword;
m_Edit_Username.GetWindowTextW(sUserName);
m_Edit_Password.GetWindowTextW(sPassword);
LPCWSTR UserName = (LPCWSTR)sUserName;
LPCWSTR Password = (LPCWSTR)sPassword;
boolean b = USR_Connect(UserName, Password);
CString str;
if (b) {
    str.Format(_T("连接已发起\n"));
};
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

### <aside>USR_DisConnect 断开连接</aside>

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
typedef boolean(_stdcall *FN_USR_DisConnect)();
```

> 调用

```pascal
if USR_DisConnect then
begin
    { 断开成功 }
end;
```

```csharp
if (USR_DisConnect())
{
    Log("已断开");
}
```

```cpp
FN_USR_DisConnect USR_DisConnect;
USR_DisConnect = (FN_USR_DisConnect)GetProcAddress(
    hUsrCloud, "USR_DisConnect");

boolean b = USR_DisConnect();
CString str;
if (b) {
    str.Format(_T("连接已断开\n"));
};
```

<br>

**函数原型:<br><br>`boolean USR_DisConnect();`**

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

## --------- 订阅消息 ---------
想要接收哪个设备发来的消息,就要订阅哪个设备的消息。

### <aside>USR_OnSubscribeAck 设置 订阅响应回调函数</aside>

> USR_OnSubscribeAck 设置 订阅响应回调函数 声明:

```pascal
  TUSR_SubscribeAckEvent = procedure(MessageID: LongInt;
   SubFunName, SubParam, ReturnCode: PWideChar); stdcall;
```

```pascal
function USR_OnSubscribeAck(OnSubscribeAck: TUSR_SubscribeAckEvent): 
Boolean; stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_SubscribeAckEvent(int messageID, 
    IntPtr SubFunName, IntPtr SubParam, IntPtr returnCode);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnSubscribeAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnSubscribeAck(
    TUSR_SubscribeAckEvent OnSubscribeAck);
```

```cpp
typedef void(_stdcall *TUSR_SubscribeAckEvent)(
    long MessageID, 
    LPCWSTR SubFunName, LPCWSTR SubParam, 
    LPCWSTR ReturnCode);
typedef boolean(_stdcall *FN_USR_OnSubscribeAck)(
    TUSR_SubscribeAckEvent OnSubscribeAck);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于判断是否订阅成功 }
procedure SubscribeAck_CBF(
    MessageID: Integer; DevId, ReturnCode: PWideChar);
var
  vsHint      : string;
begin
  vsHint := Format(
    '订阅响应:' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '函数名称:%s' + Chr(13) + Chr(10) +
    '设备ID(或用户名):%s' + Chr(13) + Chr(10) +
    '结果分别是:%s',
    [MessageID,
     WideCharToString(SubFunName),
     WideCharToString(SubParam),
     WideCharToString(ReturnCode)]);
  Writeln(vsHint);
end;
```

```pascal
{ 注册回调函数 }
USR_OnSubscribeAck( SubscribeAck_CBF );
```

```csharp
/* 自定义回调函数,用于判断订阅结果 */
private void SubscribeAck_CBF(
    int messageID,
    IntPtr SubFunName, IntPtr SubParam, IntPtr returnCode)
{
    string sSubFunName= Marshal.PtrToStringAuto(SubFunName);
    string sSubParam= Marshal.PtrToStringAuto(SubParam);
    string sReturnCode = Marshal.PtrToStringAuto(returnCode);
    string[] SubParamArray = sSubParam.Split(',');
    string[] retCodeArray = sReturnCode.Split(',');
    int len = SubParamArray.Length;

    Log("【订阅回调】");
    Log("MsgId:" + messageID.ToString());
    Log("函数名称:" + sSubFunName);
    for (int i = 0; i < len; ++i)
    {
        Log("设备ID(或用户名)：" + SubParamArray[i] +
         "  订阅结果：" + retCodeArray[i]);
    }
}
```

```csharp
TUSR_SubscribeAckEvent FSubscribeAck_CBF;
FSubscribeAck_CBF = new TUSR_SubscribeAckEvent(
     SubscribeAck_CBF );
/* 注册回调函数 */
USR_OnSubscribeAck( FSubscribeAck_CBF );
```

```cpp
/* 自定义回调函数,用于判断订阅结果 */
static void _stdcall SubscribeAck_CBF(
    long MessageID, 
    LPCWSTR SubFunName, LPCWSTR SubParam, 
    LPCWSTR ReturnCode);
void CUsrCloudDllDemoDlg::SubscribeAck_CBF(
    long MessageID, LPCWSTR SubFunName, LPCWSTR SubParam, 
    LPCWSTR ReturnCode)
{
	CString str;
	str.Format(
        _T("【订阅事件】\nMessageID：%d\n  SubFunName： %s\n 设备ID(或用户名)： %s\n 订阅结果:%s\n"), 
        MessageID, SubFunName, SubParam, ReturnCode);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */
FN_USR_OnSubscribeAck USR_OnSubscribeAck;
USR_OnSubscribeAck = (FN_USR_OnSubscribeAck)GetProcAddress(
    hUsrCloud, "USR_OnSubscribeAck");
USR_OnSubscribeAck(SubscribeAck_CBF);
```

<br>

**函数原型:<br><br>`boolean USR_OnSubscribeAck(TUSR_SubscribeAckEvent OnSubscribeAck);`**

参数|描述
----|----
OnSubscribeAck |[in] 订阅响应回调函数   [TUSR_SubscribeAckEvent 定义](#Define_TUSR_SubscribeAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_SubscribeAckEvent"></span>
**TUSR_SubscribeAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_SubscribeAckEvent)(
                long MessageID,
                LPCWSTR SubFunName,
                LPCWSTR SubParam,
                LPCWSTR ReturnCode
                );`**

参数|描述
----|----
MessageID |[out] 消息ID,一般用不到
SubFunName |[out] 函数名称,用于判断用户执行的哪个订阅函数, 得到了服务器的回应。<br>可能的取值有: <br>SubscribeDevParsed<br>SubscribeUserParsed<br>SubscribeDevRaw<br>SubscribeUserRaw
SubParam |[out] 订阅参数,多个用逗号隔开<br>SubParam值跟执行的订阅函数有关:<br>如果订阅的是"单个设备的消息", 则SubParam为设备ID;<br>如果订阅的是"用户所有设备的消息", 则SubParam为用户名
ReturnCode|[out] 返回码,表示订阅结果,多个用逗号隔开,和SubParam依次对应。

ReturnCode可能的值有:

返回码| 返回码代表的含义
---- | ----
0   | 最大QoS 0
1   | 成功 – 最大QoS 1
2   | 成功 – 最大 QoS 2
128 | Failure 失败

### <aside>USR_OnUnSubscribeAck 设置 取消订阅响应回调函数</aside>

> USR_OnUnSubscribeAck 设置 取消订阅响应回调函数 声明:

```pascal
  TUSR_UnSubscribeAckEvent = procedure(
      MessageID: LongInt; 
      UnSubFunName, UnSubParam: PWideChar); stdcall;
```

```pascal
function USR_OnUnSubscribeAck(
    OnUnSubscribeAck: TUSR_UnSubscribeAckEvent):
    stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_UnSubscribeAckEvent(
    int messageID, 
    IntPtr UnSubFunName, 
    IntPtr UnSubParam);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnUnSubscribeAck", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnUnSubscribeAck(
    TUSR_UnSubscribeAckEvent OnUnSubscribeAck);
```

```cpp
typedef void(_stdcall *TUSR_UnSubscribeAckEvent)(
    long MessageID, LPCWSTR UnSubFunName, LPCWSTR UnSubParam);
typedef boolean(_stdcall *FN_USR_OnUnSubscribeAck)(
    TUSR_UnSubscribeAckEvent OnUnSubscribeAck);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数  }
procedure UnSubscribeAck_CBF(MessageID: LongInt;
  UnSubFunName, UnSubParam: PWideChar);
begin
  FrmUsrCloudDllDemo.Log(
      '【取消订阅事件】'#13#10 + 
      'MessageID:%d'#13#10 +
      '函数名称:%s'#13#10 + 
      '设备ID(或用户名):%s',
    [MessageID, 
    WideCharToString(UnSubFunName), 
    WideCharToString(UnSubParam)]);
end;
```

```pascal
{ 注册回调函数 }
USR_OnUnSubscribeAck(UnSubscribeAck_CBF);
```

```csharp
/* 自定义回调函数 */
private void UnSubscribeAck_CBF(int messageID, 
    IntPtr UnSubFunName, 
    IntPtr UnSubParam)
{
    string sUnSubFunName = Marshal.PtrToStringAuto(
        UnSubFunName);
    string sUnSubParam = Marshal.PtrToStringAuto(
        UnSubParam);
    Log("【取消订阅事件】");
    Log("MsgId:" + messageID.ToString());
    Log("函数名称：" + sUnSubFunName);
    Log("设备ID或用户名：" + sUnSubParam);
}
```

```csharp
TUSR_UnSubscribeAckEvent FUnSubscribeAck_CBF;
FUnSubscribeAck_CBF = new TUSR_UnSubscribeAckEvent(
    UnSubscribeAck_CBF);
/* 注册回调函数 */
USR_OnUnSubscribeAck(FUnSubscribeAck_CBF);
```

```cpp
/* 自定义回调函数,用于判断订阅结果 */
static void _stdcall UnSubscribeAck_CBF(
    long MessageID, LPCWSTR UnSubFunName, LPCWSTR UnSubParam);

void CUsrCloudDllDemoDlg::UnSubscribeAck_CBF(
    long MessageID, LPCWSTR UnSubFunName, LPCWSTR UnSubParam)
{
	CString str;
	str.Format(
        _T("【取消订阅事件】\nMessageID：%d\n  UnSubFunName： %s\n 设备ID(或用户名)： %s\n"), 
        MessageID, UnSubFunName, UnSubParam);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */
FN_USR_OnUnSubscribeAck USR_OnUnSubscribeAck;
USR_OnUnSubscribeAck = (FN_USR_OnUnSubscribeAck)GetProcAddress(
    hUsrCloud, "USR_OnUnSubscribeAck");
USR_OnUnSubscribeAck(UnSubscribeAck_CBF);
```

<br>

**函数原型:<br><br>`boolean USR_OnUnSubscribeAck(TUSR_UnSubscribeAckEvent OnUnSubscribeAck);`**

参数|描述
----|----
OnUnSubscribeAck |[in] 取消订阅 响应 回调函数   [TUSR_UnSubscribeAckEvent 定义](#Define_TUSR_UnSubscribeAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_UnSubscribeAckEvent"></span>
**TUSR_UnSubscribeAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_UnSubscribeAckEvent )(
                long MessageID, 
                LPCWSTR UnSubFunName, 
                LPCWSTR UnSubParam
                );`**

参数|描述
----|----
MessageID |[out] 消息ID,一般用不到
UnSubFunName |[out] 函数名称,用于判断用户执行的哪个取消订阅函数, 得到了服务器的回应。<br>可能的取值有: <br>UnSubscribeDevParsed<br>UnSubscribeUserParsed<br>UnSubscribeDevRaw<br>UnSubscribeUserRaw
UnSubParam |[out] 取消订阅参数,多个用逗号隔开<br>UnSubParam值跟执行的取消订阅函数有关:<br>如果取消订阅的是"单个设备的消息", 则UnSubParam为设备ID;<br>如果取消订阅的是"用户所有设备的消息", 则UnSubParam为用户名

### --------- 云组态操作 ---------

### <aside>USR_SubscribeDevParsed 订阅单个设备解析后的数据</aside>

> USR_SubscribeDevParsed 订阅单个设备解析后的数据 声明:

```pascal
function USR_SubscribeDevParsed(
    DevId: PWideChar): LongInt;
    stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_SubscribeDevParsed", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_SubscribeDevParsed(string devId);
```

```cpp
typedef long(_stdcall *FN_USR_SubscribeDevParsed)(
    LPCWSTR DevId);
```

> 调用

```pascal
if USR_SubscribeDevParsed(
    PWideChar('00000000000000000001,00000000000000000002')
    ) > -1 then
begin
  { 已发起订阅两个设备00000000000000000001,00000000000000000002
    请在USR_OnSubscribeAck设置的回调函数中判断订阅结果  }
end;
```

```csharp
int iMsgId = USR_SubscribeDevParsed("00000000000000000001");
if(iMsgId > -1)
{
    Log("订阅设备00000000000000000001 已发起");  
}
```

```cpp
FN_USR_SubscribeDevParsed USR_SubscribeDevParsed;
USR_SubscribeDevParsed = (FN_USR_SubscribeDevParsed)GetProcAddress(
    hUsrCloud, "USR_SubscribeDevParsed");

CString sDevId;
m_Edit_SubParsedDevId.GetWindowTextW(sDevId);
LPCWSTR DevId = (LPCWSTR)sDevId;
long iMsgId = USR_SubscribeDevParsed(DevId);
CString str;
str.Format(
    _T("USR_SubscribeDevParsed 订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_SubscribeDevParsed(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要订阅哪个设备发来的消息。<br>**如果要订阅多个,请用逗号隔开;<br>如果要订阅帐号下所有的设备的消息,请使用USR_SubscribeUserParsed函数。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnSubscribeAck设置的回调函数来判断。

### <aside>USR_SubscribeUserParsed 订阅账户下所有设备解析后的数据</aside>

> USR_SubscribeUserParsed 订阅账户下所有设备解析后的数据 声明:

```pascal
function USR_SubscribeUserParsed(Username: PWideChar): LongInt;
 stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_SubscribeUserParsed", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_SubscribeUserParsed(string Username);
```

```cpp
typedef long(_stdcall *FN_USR_SubscribeUserParsed)(
    LPCWSTR Username);
```

> 调用

```pascal
if USR_SubscribeUserParsed(
    PWideChar('sdktest')
    ) > -1 then
begin
  { 已发起订阅sdktest
    请在USR_OnSubscribeAck设置的回调函数中判断订阅结果  }
end;
```

```csharp
int iMsgId = USR_SubscribeUserParsed("sdktest");
if(iMsgId > -1)
{
    Log("订阅账户sdktest 已发起");  
}
```

```cpp
FN_USR_SubscribeUserParsed USR_SubscribeUserParsed;
USR_SubscribeUserParsed = (FN_USR_SubscribeUserParsed)GetProcAddress(
    hUsrCloud, "USR_SubscribeUserParsed");

CString sUserName;
m_Edit_SubParsedUsername.GetWindowTextW(sUserName);
LPCWSTR UserName = (LPCWSTR)sUserName;
long iMsgId = USR_SubscribeUserParsed(UserName);
CString str;
str.Format(
    _T("USR_SubscribeUserParsed 订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_SubscribeUserParsed(LPCWSTR Username);`**

参数| 描述
---- | ----
Username|[in] 用户名。指定要订阅哪个用户的设备发来的消息。如果要订阅多个,请用逗号隔开

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnSubscribeAck设置的回调函数来判断。


### <aside>USR_UnSubscribeDevParsed 取消订阅单个设备解析后的数据</aside>

> USR_UnSubscribeDevParsed 取消订阅单个设备解析后的数据 声明:

```pascal
function USR_UnSubscribeDevParsed(
    DevId: PWideChar): LongInt;
    stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_UnSubscribeDevParsed", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_UnSubscribeDevParsed(string DevId);
```

```cpp
typedef long(_stdcall *FN_USR_UnSubscribeDevParsed)(
    LPCWSTR DevId);
```

> 调用

```pascal
if USR_UnSubscribeDevParsed(
    PWideChar('00000000000000000001,00000000000000000002')
    ) > -1 then
begin
  { 已发起取消订阅两个设备00000000000000000001,00000000000000000002
    请在USR_OnUnSubscribeAck设置的回调函数中判断取消订阅结果  }
end;
```

```csharp
int iMsgId = USR_UnSubscribeDevParsed("00000000000000000001");
if(iMsgId > -1)
{
    Log("取消订阅设备00000000000000000001 已发起");  
}
```

```cpp
FN_USR_UnSubscribeDevParsed USR_UnSubscribeDevParsed;
USR_UnSubscribeDevParsed = (FN_USR_UnSubscribeDevParsed)GetProcAddress(
    hUsrCloud, "USR_UnSubscribeDevParsed");

CString sDevId;
m_Edit_SubParsedDevId.GetWindowTextW(sDevId);
LPCWSTR DevId = (LPCWSTR)sDevId;
long iMsgId = USR_UnSubscribeDevParsed(DevId);
CString str;
str.Format(
    _T("USR_UnSubscribeDevParsed 取消订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_UnSubscribeDevParsed(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要取消订阅哪个设备发来的消息。<br>**如果要取消订阅多个,请用逗号隔开;<br>如果要取消订阅帐号下所有的设备的消息,请使用USR_UnSubscribeUserParsed函数。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终取消订阅结果要通过USR_OnUnSubscribeAck设置的回调函数来判断。

### <aside>USR_UnSubscribeUserParsed 取消订阅账户下所有设备解析后的数据</aside>

> USR_UnSubscribeUserParsed 取消订阅账户下所有设备解析后的数据 声明:

```pascal
function USR_UnSubscribeUserParsed(Username: PWideChar): LongInt;
 stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_UnSubscribeUserParsed", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_UnSubscribeUserParsed(
    string Username);
```

```cpp
typedef long(_stdcall *FN_USR_UnSubscribeUserParsed)(
    LPCWSTR Username);
```

> 调用

```pascal
if USR_UnSubscribeUserParsed(
    PWideChar('sdktest')
    ) > -1 then
begin
  { 已发起取消订阅sdktest
    请在USR_OnUnSubscribeAck设置的回调函数中判断取消订阅结果  }
end;
```

```csharp
int iMsgId = USR_UnSubscribeUserParsed("sdktest");
if(iMsgId > -1)
{
    Log("取消订阅账户sdktest 已发起");  
}
```

```cpp
FN_USR_UnSubscribeUserParsed USR_UnSubscribeUserParsed;
USR_UnSubscribeUserParsed = (FN_USR_UnSubscribeUserParsed)GetProcAddress(
    hUsrCloud, "USR_UnSubscribeUserParsed");

CString sUserName;
m_Edit_SubParsedUsername.GetWindowTextW(sUserName);
LPCWSTR UserName = (LPCWSTR)sUserName;
long iMsgId = USR_UnSubscribeUserParsed(UserName);
CString str;
str.Format(
    _T("USR_UnSubscribeUserParsed 取消订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_UnSubscribeUserParsed(LPCWSTR Username);`**

参数| 描述
---- | ----
Username|[in] 用户名。指定要取消订阅哪个用户的设备发来的消息。如果要取消订阅多个,请用逗号隔开

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终取消订阅结果要通过USR_OnUnSubscribeAck设置的回调函数来判断。

### --------- 云交换机操作 ---------

### <aside>USR_SubscribeDevRaw 订阅单个设备原始数据流</aside>

> USR_SubscribeDevRaw 订阅单个设备原始数据流 声明:

```pascal
function USR_SubscribeDevRaw(DevId: PWideChar): LongInt;
 stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_SubscribeDevRaw", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_SubscribeDevRaw(string devId);
```

```cpp
typedef long(_stdcall *FN_USR_SubscribeDevRaw)(
    LPCWSTR DevId);

```

> 调用

```pascal
if USR_SubscribeDevRaw(
    PWideChar('00000000000000000001,00000000000000000002')
    ) > -1 then
begin
  { 已发起订阅两个设备00000000000000000001,00000000000000000002
    请在USR_OnSubscribeAck设置的回调函数中判断订阅结果  }
end;
```

```csharp
int iMsgId = USR_SubscribeDevRaw("00000000000000000001");
if(iMsgId > -1)
{
    Log("订阅设备00000000000000000001 已发起");  
}
```

```cpp
FN_USR_SubscribeDevRaw USR_SubscribeDevRaw;
USR_SubscribeDevRaw = (FN_USR_SubscribeDevRaw)GetProcAddress(
    hUsrCloud, "USR_SubscribeDevRaw");

CString sDevId;
m_Edit_SubRawDevId.GetWindowTextW(sDevId);
LPCWSTR DevId = (LPCWSTR)sDevId;
long iMsgId = USR_SubscribeDevRaw(DevId);
CString str;
str.Format(
    _T("USR_SubscribeDevRaw 订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_SubscribeDevRaw(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要订阅哪个设备发来的消息。<br>**如果要订阅多个,请用逗号隔开;<br>如果要订阅帐号下所有的设备的消息,请使用USR_SubscribeUserRaw函数。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnSubscribeAck设置的回调函数来判断。

### <aside>USR_SubscribeUserRaw 订阅账户下所有设备原始数据流</aside>

> USR_SubscribeUserRaw 订阅账户下所有设备原始数据流 声明:

```pascal
function USR_SubscribeUserRaw(
    Username: PWideChar): LongInt;
    stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_SubscribeUserRaw", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_SubscribeUserRaw(
    string Username);
```

```cpp
typedef long(_stdcall *FN_USR_SubscribeUserRaw)(
    LPCWSTR Username);
```

> 调用

```pascal
if USR_SubscribeUserRaw(
    PWideChar('sdktest')
    ) > -1 then
begin
  { 已发起订阅sdktest
    请在USR_OnSubscribeAck设置的回调函数中判断订阅结果  }
end;
```

```csharp
int iMsgId = USR_SubscribeUserRaw("sdktest");
if(iMsgId > -1)
{
    Log("订阅账户sdktest 已发起");  
}
```

```cpp
FN_USR_SubscribeUserRaw USR_SubscribeUserRaw;
USR_SubscribeUserRaw = (FN_USR_SubscribeUserRaw)GetProcAddress(
    hUsrCloud, "USR_SubscribeUserRaw");

CString sUserName;
m_Edit_SubRawUsername.GetWindowTextW(sUserName);
LPCWSTR UserName = (LPCWSTR)sUserName;
long iMsgId = USR_SubscribeUserRaw(UserName);
CString str;
str.Format(
    _T("USR_SubscribeUserRaw 订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_SubscribeUserRaw(LPCWSTR Username);`**

参数| 描述
---- | ----
Username|[in] 用户名。指定要订阅哪个用户的设备发来的消息。如果要订阅多个,请用逗号隔开

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终订阅结果要通过USR_OnSubscribeAck设置的回调函数来判断。


### <aside>USR_UnSubscribeDevRaw 取消订阅单个设备原始数据流</aside>

> USR_UnSubscribeDevRaw 取消订阅单个设备原始数据流 声明:

```pascal
function USR_UnSubscribeDevRaw(DevId: PWideChar): LongInt;
 stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_UnSubscribeDevRaw", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_UnSubscribeDevRaw(
    string DevId);
```

```cpp
typedef long(_stdcall *FN_USR_UnSubscribeDevRaw)(
    LPCWSTR DevId);
```

> 调用

```pascal
if USR_UnSubscribeDevRaw(
    PWideChar('00000000000000000001,00000000000000000002')
    ) > -1 then
begin
  { 已发起取消订阅两个设备00000000000000000001,00000000000000000002
    请在USR_OnUnSubscribeAck设置的回调函数中判断取消订阅结果  }
end;
```

```csharp
int iMsgId = USR_UnSubscribeDevRaw("00000000000000000001");
if(iMsgId > -1)
{
    Log("取消订阅设备00000000000000000001 已发起");  
}
```

```cpp
FN_USR_UnSubscribeDevRaw USR_UnSubscribeDevRaw;
USR_UnSubscribeDevRaw = (FN_USR_UnSubscribeDevRaw)GetProcAddress(
    hUsrCloud, "USR_UnSubscribeDevRaw");

CString sDevId;
m_Edit_SubRawDevId.GetWindowTextW(sDevId);
LPCWSTR DevId = (LPCWSTR)sDevId;
long iMsgId = USR_UnSubscribeDevRaw(DevId);
CString str;
str.Format(
    _T("USR_UnSubscribeDevRaw 取消订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_UnSubscribeDevRaw(LPCWSTR DevId);`**

参数| 描述
---- | ----
DevId|[in] 设备ID。指定要取消订阅哪个设备发来的消息。<br>**如果要取消订阅多个,请用逗号隔开;<br>如果要取消订阅帐号下所有的设备的消息,请使用USR_UnSubscribeUserRaw函数。**

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终取消订阅结果要通过USR_OnUnSubscribeAck设置的回调函数来判断。

### <aside>USR_UnSubscribeUserRaw 取消订阅账户下所有设备原始数据流</aside>

> USR_UnSubscribeUserRaw 取消订阅账户下所有设备原始数据流 声明:

```pascal
function USR_UnSubscribeUserRaw(
    Username: PWideChar): LongInt;
 stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_UnSubscribeUserRaw", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_UnSubscribeUserRaw(
    string Username);
```

```cpp
typedef long(_stdcall *FN_USR_UnSubscribeUserRaw)(
    LPCWSTR Username);
```

> 调用

```pascal
if USR_UnSubscribeUserRaw(
    PWideChar('sdktest')
    ) > -1 then
begin
  { 已发起取消订阅sdktest
    请在USR_OnUnSubscribeAck设置的回调函数中判断取消订阅结果  }
end;
```

```csharp
int iMsgId = USR_UnSubscribeUserRaw("sdktest");
if(iMsgId > -1)
{
    Log("取消订阅账户sdktest 已发起");  
}
```

```cpp
FN_USR_UnSubscribeUserRaw USR_UnSubscribeUserRaw;
USR_UnSubscribeUserRaw = (FN_USR_UnSubscribeUserRaw)GetProcAddress(
    hUsrCloud, "USR_UnSubscribeUserRaw");

CString sUserName;
m_Edit_SubRawUsername.GetWindowTextW(sUserName);
LPCWSTR UserName = (LPCWSTR)sUserName;
long iMsgId = USR_UnSubscribeUserRaw(UserName);
CString str;
str.Format(
    _T("USR_UnSubscribeUserRaw 取消订阅已发起\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_UnSubscribeUserRaw(LPCWSTR Username);`**

参数| 描述
---- | ----
Username|[in] 用户名。指定要取消订阅哪个用户的设备发来的消息。如果要取消订阅多个,请用逗号隔开

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID,收到消息ID只是说明消息发到服务器上去了,最终取消订阅结果要通过USR_OnUnSubscribeAck设置的回调函数来判断。

## --------- 推送消息 ---------

### <aside>USR_OnPubAck 设置 推送响应回调函数</aside>

> USR_OnPubAck 设置 推送响应回调函数 声明:

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
public static extern bool USR_OnPubAck(
    TUSR_PubAckEvent OnPubAck);
```

```cpp
typedef void(_stdcall *TUSR_PubAckEvent)(long MessageID);
typedef boolean(_stdcall *FN_USR_OnPubAck)(
    TUSR_PubAckEvent OnPubAck);
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
/* 自定义回调函数,用于判断是否推送成功 */
static void _stdcall PubAck_CBF(long MessageID);

void CUsrCloudDllDemoDlg::PubAck_CBF(long MessageID)
{
	CString str;
	str.Format(_T("【推送事件】\n MessageID：%d\n"), MessageID);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */
FN_USR_OnPubAck USR_OnPubAck;
USR_OnPubAck = (FN_USR_OnPubAck)GetProcAddress(
    hUsrCloud, "USR_OnPubAck");
USR_OnPubAck(PubAck_CBF);
```

<br>

**函数原型:<br><br>`boolean USR_OnPubAck(TUSR_PubAckEvent OnPubAck);`**

参数|描述
----|----
OnPubAck |[in] 推送响应回调函数   [TUSR_PubAckEvent 定义](#Define_TUSR_TUSR_PubAckEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_TUSR_PubAckEvent"></span>
**TUSR_PubAckEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_PubAckEvent )(
                long MessageID
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。用于判断推送的哪条消息得到服务器的响应了。

### --------- 云组态操作 ---------

### <aside>USR_PublishParsedSetDataPoint 设置单台设备数据点值</aside>

> USR_PublishParsedSetDataPoint 设置单台设备数据点值 声明:

```pascal
function USR_PublishParsedSetDataPoint(DevId, PointId, Value: PWideChar):
  LongInt; stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_PublishParsedSetDataPoint", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_PublishParsedSetDataPoint(
    string DevId, string PointId, string Value);
```

```cpp
typedef long(_stdcall *FN_USR_PublishParsedSetDataPoint)(
    LPCWSTR DevId, LPCWSTR PointId, LPCWSTR Value);
```

> 调用

```pascal
var
  viMsgId           : Integer;
begin
  viMsgId := USR_PublishParsedSetDataPoint(
      PWideChar('00000000000000000001'),
      PWideChar('118'),
      PWideChar('1234')
      );
  if viMsgId > -1 then
    Writeln('消息已推送 MsgId:' + IntToStr(viMsgId));
end;
```

```csharp
int iMsgId = USR_PublishParsedSetDataPoint(
    "00000000000000000001", "118", "1234");
if (iMsgId > -1) 
{
    Log("消息已推送 MsgId:" + iMsgId.ToString());
}
```

```cpp
FN_USR_PublishParsedSetDataPoint USR_PublishParsedSetDataPoint;
USR_PublishParsedSetDataPoint = (FN_USR_PublishParsedSetDataPoint)GetProcAddress(
    hUsrCloud, "USR_PublishParsedSetDataPoint");

CString sDevId, sPointId, sValue;
m_Edit_PubParsedDevId.GetWindowTextW(sDevId);
m_Edit_PubParsedPointId.GetWindowTextW(sPointId);
m_Edit_PubParsedValueS.GetWindowTextW(sValue);
LPCWSTR DevId = (LPCWSTR)sDevId;
LPCWSTR PointId = (LPCWSTR)sPointId;
LPCWSTR Value = (LPCWSTR)sValue;
long iMsgId = USR_PublishParsedSetDataPoint(
    DevId, PointId, Value);
CString str;
str.Format(_T("消息已推送\n MsgId:%d\n"), iMsgId); 
```

<br>

**函数原型:<br><br>`long USR_PublishParsedSetDataPoint(LPCWSTR DevId, LPCWSTR PointId, LPCWSTR Value);`**

参数| 描述
---- | ----
DevId  | [in] 设备ID,指定要把数据发给哪个设备, **只能填一个。**
PointId | [in] 数据点ID
Value | [in] 数据点值

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

### <aside>USR_PublishParsedQueryDataPoint 查询单台设备数据点值</aside>

> USR_PublishParsedQueryDataPoint 查询单台设备数据点值 声明:

```pascal
function USR_PublishParsedQueryDataPoint(
    DevId, PointId: PWideChar): LongInt; 
stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_PublishParsedQueryDataPoint", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_PublishParsedQueryDataPoint(
    string DevId, string PointId);
```

```cpp
typedef long(_stdcall *FN_USR_PublishParsedQueryDataPoint)(
    LPCWSTR DevId, LPCWSTR PointId);
```

> 调用

```pascal
var
  viMsgId           : Integer;
begin
  viMsgId := USR_PublishParsedQueryDataPoint(
      PWideChar('00000000000000000001'),
      PWideChar('118')
      );
  if viMsgId > -1 then
    Writeln('消息已推送 MsgId:' + IntToStr(viMsgId));
end;
```

```csharp
int iMsgId = USR_PublishParsedQueryDataPoint(
    "00000000000000000001", "118");
if (iMsgId > -1) 
{
    Log("消息已推送 MsgId:" + iMsgId.ToString());
}
```

```cpp
FN_USR_PublishParsedQueryDataPoint USR_PublishParsedQueryDataPoint;
USR_PublishParsedQueryDataPoint = (FN_USR_PublishParsedQueryDataPoint)GetProcAddress(
    hUsrCloud, "USR_PublishParsedQueryDataPoint");

CString sDevId, sPointId;
m_Edit_PubParsedDevId.GetWindowTextW(sDevId);
m_Edit_PubParsedPointId.GetWindowTextW(sPointId);
LPCWSTR DevId = (LPCWSTR)sDevId;
LPCWSTR PointId = (LPCWSTR)sPointId;
long iMsgId = USR_PublishParsedQueryDataPoint(
    DevId, PointId);
CString str;
str.Format(_T("消息已推送\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_PublishParsedQueryDataPoint(LPCWSTR DevId, LPCWSTR PointId);`**

参数| 描述
---- | ----
DevId  | [in] 设备ID,指定要把数据发给哪个设备, **只能填一个。**
PointId | [in] 数据点ID

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

### --------- 云交换机操作 ---------

### <aside>USR_PublishRawToDev 向单台设备推送原始数据流</aside>

> USR_PublishRawToDev 向单台设备推送原始数据流 声明:

```pascal
function USR_PublishRawToDev(
    DevId: PWideChar; pData: PByte; DataLen: Integer
    ) : LongInt; stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_PublishRawToDev", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_PublishRawToDev(
    string DevId, byte[] pData, int DataLen);
```

```cpp
typedef long(_stdcall *FN_USR_PublishRawToDev)(
    LPCWSTR DevId, void *pData, long DataLen);
```

> 调用

```pascal
var
  viMsgId           : Integer;
  vsData            : string;
begin
  vsData := 'abcd';
  viMsgId := USR_PublishRawToDev(
      PWideChar('00000000000000000001'),
      @vsData[1], Length(vsData)
      );
  if viMsgId > -1 then
    Writeln('消息已推送 MsgId:' + IntToStr(viMsgId));
end;
```

```csharp
byte[] byteArray = new byte[] { 0x01, 0x02, 0x03 };
int iMsgId = USR_PublishRawToDev("00000000000000000001", 
                         byteArray, 
                         byteArray.Length);
if (iMsgId > -1) 
{
    Log("消息已推送 MsgId:" + iMsgId.ToString());
}
```

```cpp
FN_USR_PublishRawToDev USR_PublishRawToDev;
USR_PublishRawToDev = (FN_USR_PublishRawToDev)GetProcAddress(
    hUsrCloud, "USR_PublishRawToDev");

CString sDevId, sData;
m_Edit_PubRawDevId.GetWindowTextW(sDevId);
m_RichEdit_PubRaw.GetWindowTextW(sData);
LPCWSTR DevId = (LPCWSTR)sDevId;
byte buf[1024];
int len = 0;
if (m_Check_PubRaw.GetCheck()) {
    len = HexStr2Buf(sData, buf);
}
else
{
    len = WideCharToMultiByte(CP_ACP, 0, sData, -1, NULL, 0, NULL, NULL) - 1;
    WideCharToMultiByte(CP_ACP, 0, sData, -1, (LPSTR)&buf[0], len, NULL, NULL);
};
long iMsgId = USR_PublishRawToDev(DevId, &buf[0], len);
CString str;
str.Format(_T("消息已推送\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_PublishRawToDev(LPCWSTR DevId, void *pData,long DataLen);`**

参数| 描述
---- | ----
DevId  | [in] 设备ID,指定要把数据发给哪个设备, **只能填一个。**
pData | [in] 数据
DataLen | [in] 数据长度

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

### <aside>USR_PublishRawToUser 向账户下所有设备推送原始数据流</aside>

> USR_PublishRawToUser 向账户下所有设备推送原始数据流 声明:

```pascal
function USR_PublishRawToUser(
    Username: PWideChar; pData: PByte; DataLen: Integer
    ) : LongInt; stdcall; external 'UsrCloud.dll';
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_PublishRawToUser", 
    CallingConvention = CallingConvention.StdCall)]
public static extern int USR_PublishRawToUser(
    string Username, byte[] pData, int DataLen);
```

```cpp
typedef long(_stdcall *FN_USR_PublishRawToUser)(
    LPCWSTR Username, void *pData, long DataLen);
```

> 调用

```pascal
var
  viMsgId           : Integer;
  vsData            : string;
begin
  vsData := 'abcd';
  viMsgId := USR_PublishRawToUser(
      PWideChar('sdktest'),
      @vsData[1], Length(vsData)
      );
  if viMsgId > -1 then
    Writeln('消息已推送 MsgId:' + IntToStr(viMsgId));
end;
```

```csharp
byte[] byteArray = new byte[] { 0x01, 0x02, 0x03 };
int iMsgId = USR_PublishRawToUser("sdktest", 
                         byteArray, 
                         byteArray.Length);
if (iMsgId > -1) 
{
    Log("消息已推送 MsgId:" + iMsgId.ToString());
}
```

```cpp
FN_USR_PublishRawToUser USR_PublishRawToUser;
USR_PublishRawToUser = (FN_USR_PublishRawToUser)GetProcAddress(
    hUsrCloud, "USR_PublishRawToUser");

CString sUsername, sData;
m_Edit_PubRawUsername.GetWindowTextW(sUsername);
m_RichEdit_PubRaw.GetWindowTextW(sData);
LPCWSTR Username = (LPCWSTR)sUsername;
byte buf[1024];
int len = 0;
if (m_Check_PubRaw.GetCheck()) {
    len = HexStr2Buf(sData, buf);
}
else
{
    len = WideCharToMultiByte(CP_ACP, 0, sData, -1, NULL, 0, NULL, NULL) - 1;
    WideCharToMultiByte(CP_ACP, 0, sData, -1, (LPSTR)&buf[0], len, NULL, NULL);
};
long iMsgId = USR_PublishRawToUser(Username, &buf[0], len);
CString str;
str.Format(_T("消息已推送\n MsgId:%d\n"), iMsgId);
```

<br>

**函数原型:<br><br>`long USR_PublishRawToUser(LPCWSTR Username, void *pData,long DataLen);`**

参数| 描述
---- | ----
Username  | [in] 用户名,指定要把数据发给哪个用户的设备, **只能填一个。**
pData | [in] 数据
DataLen | [in] 数据长度

返回值| 描述
---- | ----
boolean|失败返回: -1 ;<br>成功返回: 消息ID。

## --------- 接收消息 ---------

### --------- 云组态操作 ---------

### <aside>USR_OnRcvParsedDataPointPush 设置 接收设备数据点值推送回调函数</aside>

> USR_OnRcvParsedDataPointPush 设置 接收设备数据点值推送回调函数 声明:

```pascal
TUSR_RcvParsedEvent = procedure(
    MessageID: LongInt; DevId, JsonStr:PWideChar
    ); stdcall;
```

```pascal
function USR_OnRcvParsedDataPointPush(
    OnRcvParsed: TUSR_RcvParsedEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_RcvParsedEvent(
    int MessageID, IntPtr DevId, IntPtr JsonStr);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnRcvParsedDataPointPush", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnRcvParsedDataPointPush(
    TUSR_RcvParsedEvent OnRcvParsed);
```

```cpp
typedef void(_stdcall *TUSR_RcvParsedEvent)(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);
typedef boolean(_stdcall *FN_USR_OnRcvParsedDataPointPush)(
    TUSR_RcvParsedEvent OnRcvParsed);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于接收数据点值推送  }
procedure RcvParsedDataPointPush_CBF(
    MessageID: LongInt; DevId, JsonStr: PWideChar);
var
  vsHint : string;
begin
  vsHint := Format(
    '【设备数据点值推送事件】' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID:%s' + Chr(13) + Chr(10) +
    'JSON数据:%s',
    [MessageID,
     WideCharToString(DevId),
     WideCharToString(JsonStr)]);

  Writeln(vsHint);
end;
```

```pascal
{ 注册回调函数 }
USR_OnRcvParsedDataPointPush(
    RcvParsedDataPointPush_CBF);
```

```csharp
/* 自定义回调函数,用于接收数据点值推送 */
private void RcvParsedDataPointPush_CBF(
    int messageID, IntPtr DevId, IntPtr JsonStr)
{
    string sDevId = Marshal.PtrToStringAuto(DevId);
    string sJsonStr = Marshal.PtrToStringAuto(JsonStr);
    Log("【设备数据点值推送事件】");
    Log("设备ID   : " + sDevId);
    Log("MsgId    : " + messageID.ToString());
    Log("JSON数据: " + sJsonStr);
}
```

```csharp
TUSR_RcvParsedEvent FRcvParsedDataPointPush_CBF;
FRcvParsedDataPointPush_CBF = new TUSR_RcvParsedEvent(
    RcvParsedDataPointPush_CBF);
/* 注册回调函数 */
USR_OnRcvParsedDataPointPush(
    FRcvParsedDataPointPush_CBF);
```

```cpp
/* 自定义回调函数,用于接收数据点值推送 */
static void _stdcall RcvParsedDataPointPush_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);

void CUsrCloudDllDemoDlg::RcvParsedDataPointPush_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr)
{
	CString str;
	str.Format(
        _T("【接收设备数据点推送事件】\n MessageID：%d\n DevId：%s\n JsonStr：%s\n"), 
        MessageID, DevId, JsonStr);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */

FN_USR_OnRcvParsedDataPointPush USR_OnRcvParsedDataPointPush;  
USR_OnRcvParsedDataPointPush = (FN_USR_OnRcvParsedDataPointPush)GetProcAddress(
    hUsrCloud, "USR_OnRcvParsedDataPointPush");
USR_OnRcvParsedDataPointPush(RcvParsedDataPointPush_CBF);
```

> 设备数据点值推送 JSON数据格式

    {
        "dataPoints": [
            {
                "pointId": "123", //数据点 id
                "value":"42.12" //数据点值（整形/浮点/布尔型）
            },
            {………}
        ],
        "devName":"123"
    }

<br>

**函数原型:<br><br>`boolean USR_OnRcvParsedDataPointPush(TUSR_RcvParsedEvent OnRcvParsed);`**

参数|描述
----|----
OnRcvParsed |[in] 接收设备数据点值推送回调函数   [TUSR_RcvParsedEvent 定义](#Define_TUSR_RcvParsedEvent)<br>[JSON格式](#Define_Json_dataPoints)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

### <aside>USR_OnRcvParsedDevStatusPush 设置 接收设备上下线推送回调函数</aside>

> USR_OnRcvParsedDevStatusPush 设置 接收设备上下线推送回调函数 声明:

```pascal
TUSR_RcvParsedEvent = procedure(
    MessageID: LongInt; DevId, JsonStr:PWideChar
    ); stdcall;
```

```pascal
function USR_OnRcvParsedDevStatusPush(
    OnRcvParsed: TUSR_RcvParsedEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_RcvParsedEvent(
    int MessageID, IntPtr DevId, IntPtr JsonStr);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnRcvParsedDevStatusPush", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnRcvParsedDevStatusPush(
    TUSR_RcvParsedEvent OnRcvParsed);
```

```cpp
typedef void(_stdcall *TUSR_RcvParsedEvent)(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);
typedef boolean(_stdcall *FN_USR_OnRcvParsedDevStatusPush)(
    TUSR_RcvParsedEvent OnRcvParsed);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于接收上下线推送  }
procedure RcvParsedDevStatusPush_CBF(MessageID: LongInt;
  DevId, JsonStr: PWideChar);
var
  vsHint : string;
begin
  vsHint := Format(
    '【设备上下线推送事件】' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID:%s' + Chr(13) + Chr(10) +
    'JSON数据:%s',
    [MessageID,
     WideCharToString(DevId),
     WideCharToString(JsonStr)]);

  Writeln(vsHint);
end;
```

```pascal
{ 注册回调函数 }
USR_OnRcvParsedDevStatusPush(
    RcvParsedDevStatusPush_CBF);
```

```csharp
/* 自定义回调函数,用于接收上下线推送 */
private void RcvParsedDevStatusPush_CBF(
    int messageID, IntPtr DevId, IntPtr JsonStr)
{
    string sDevId = Marshal.PtrToStringAuto(DevId);
    string sJsonStr = Marshal.PtrToStringAuto(JsonStr);
    Log("【设备上下线推送事件】");
    Log("设备ID   : " + sDevId);
    Log("MsgId    : " + messageID.ToString());
    Log("JSON数据: " + sJsonStr);
}
```

```csharp
TUSR_RcvParsedEvent FRcvParsedDevStatusPush_CBF;
FRcvParsedDevStatusPush_CBF = new TUSR_RcvParsedEvent(
    RcvParsedDevStatusPush_CBF);
/* 注册回调函数 */
USR_OnRcvParsedDevStatusPush(
    FRcvParsedDevStatusPush_CBF);
```

```cpp
/* 自定义回调函数,用于接收上下线推送 */
static void _stdcall RcvParsedDevStatusPush_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);

void CUsrCloudDllDemoDlg::RcvParsedDevStatusPush_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr)
{
	CString str;
	str.Format(
        _T("【接收设备在线状态推送事件】\n MessageID：%d\n DevId：%s\n JsonStr：%s\n"), 
        MessageID, DevId, JsonStr);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */

FN_USR_OnRcvParsedDevStatusPush USR_OnRcvParsedDevStatusPush;  
USR_OnRcvParsedDevStatusPush = (FN_USR_OnRcvParsedDevStatusPush)GetProcAddress(
    hUsrCloud, "USR_OnRcvParsedDevStatusPush");
USR_OnRcvParsedDevStatusPush(RcvParsedDevStatusPush_CBF);
```

> 设备上下线推送 JSON数据格式

    {
        "devStatus":
        {
            "devName":"123",
            "status":1 //1 上线 0 下线
        }
    }

<br>

**函数原型:<br><br>`boolean USR_OnRcvParsedDevStatusPush(TUSR_RcvParsedEvent OnRcvParsed);`**

参数|描述
----|----
OnRcvParsed |[in] 接收设备上下线推送回调函数   [TUSR_RcvParsedEvent 定义](#Define_TUSR_RcvParsedEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false


### <aside>USR_OnRcvParsedDevAlarmPush 设置 接收设备报警推送回调函数</aside>

> USR_OnRcvParsedDevAlarmPush 设置 接收设备报警推送回调函数 声明:

```pascal
TUSR_RcvParsedEvent = procedure(
    MessageID: LongInt; DevId, JsonStr:PWideChar
    ); stdcall;
```

```pascal
function USR_OnRcvParsedDevAlarmPush(
    OnRcvParsed: TUSR_RcvParsedEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_RcvParsedEvent(int MessageID, 
    IntPtr DevId, IntPtr JsonStr);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnRcvParsedDevAlarmPush", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnRcvParsedDevAlarmPush(
    TUSR_RcvParsedEvent OnRcvParsed);
```

```cpp
typedef void(_stdcall *TUSR_RcvParsedEvent)(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);
typedef boolean(_stdcall *FN_USR_OnRcvParsedDevAlarmPush)(
    TUSR_RcvParsedEvent OnRcvParsed);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于接收报警推送  }
procedure RcvParsedDevAlarmPush_CBF(
    MessageID: LongInt; DevId, JsonStr: PWideChar);
var
  vsHint : string;
begin
  vsHint := Format(
    '【设备报警推送事件】' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID:%s' + Chr(13) + Chr(10) +
    'JSON数据:%s',
    [MessageID,
     WideCharToString(DevId),
     WideCharToString(JsonStr)]);

  Writeln(vsHint);
end;
```

```pascal
{ 注册回调函数 }
USR_OnRcvParsedDevAlarmPush(
    RcvParsedDevAlarmPush_CBF);
```

```csharp
/* 自定义回调函数,用于接收报警推送 */
private void RcvParsedDevAlarmPush_CBF(
    int messageID, IntPtr DevId, IntPtr JsonStr)
{
    string sDevId = Marshal.PtrToStringAuto(DevId);
    string sJsonStr = Marshal.PtrToStringAuto(JsonStr);
    Log("【设备报警推送事件】");
    Log("设备ID   : " + sDevId);
    Log("MsgId    : " + messageID.ToString());
    Log("JSON数据: " + sJsonStr);
}
```

```csharp
TUSR_RcvParsedEvent FRcvParsedDevAlarmPush_CBF;
FRcvParsedDevAlarmPush_CBF = new TUSR_RcvParsedEvent(
    RcvParsedDevAlarmPush_CBF);
/* 注册回调函数 */
USR_OnRcvParsedDevAlarmPush(
    FRcvParsedDevAlarmPush_CBF);
```

```cpp
/* 自定义回调函数,用于接收报警推送 */
static void _stdcall RcvParsedDevAlarmPush_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);

void CUsrCloudDllDemoDlg::RcvParsedDevAlarmPush_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr)
{
	CString str;
	str.Format(
        _T("【接收设备报警推送事件】\n MessageID：%d\n DevId：%s\n JsonStr：%s\n"), 
        MessageID, DevId, JsonStr);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */

FN_USR_OnRcvParsedDevAlarmPush USR_OnRcvParsedDevAlarmPush;  
USR_OnRcvParsedDevAlarmPush = (FN_USR_OnRcvParsedDevAlarmPush)GetProcAddress(
    hUsrCloud, "USR_OnRcvParsedDevAlarmPush");
USR_OnRcvParsedDevAlarmPush(RcvParsedDevAlarmPush_CBF);
```

> 设备报警推送 JSON数据格式

    {
        "devAlarm": {
            "devName":"123",
            " pointId":"123", //数据点 id
            "dataName":"温度", //数据点名称
            "value":"12.11", //触发报警值
            "alarmValue":"12.00", //设定的报警值
            "alarmCondition":"0", //报警条件:
                                //开关 ON(0) , 开关 OFF(1) ,
                                //数值低于(2) , 数值高于(3) ,
                                //数值介于(4) ,
                                //数值高于 max 低于 min(5)
            "alarmState":"1"     //报警状态  1开始报警  0恢复正常
        }
    }

<br>

**函数原型:<br><br>`boolean USR_OnRcvParsedDevAlarmPush(TUSR_RcvParsedEvent OnRcvParsed);`**

参数|描述
----|----
OnRcvParsed |[in] 接收设备报警推送回调函数   [TUSR_RcvParsedEvent 定义](#Define_TUSR_RcvParsedEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false


### <aside>USR_OnRcvParsedOptionResponseReturn 设置 接收设备数据点操作应答回调函数</aside>

用 USR_PublishParsedSetDataPoint 设置设备数据点值时，返回的操作结果（目前只用于 COAP类型的设备，并且设备作为 modbus 主机）。

> USR_OnRcvParsedOptionResponseReturn 设置 接收设备数据点操作应答回调函数 声明:

```pascal
TUSR_RcvParsedEvent = procedure(
    MessageID: LongInt; DevId, JsonStr:PWideChar
    ); stdcall;
```

```pascal
function USR_OnRcvParsedOptionResponseReturn(
    OnRcvParsed: TUSR_RcvParsedEvent): Boolean; 
stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_RcvParsedEvent(int MessageID, 
    IntPtr DevId, IntPtr JsonStr);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnRcvParsedOptionResponseReturn", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnRcvParsedOptionResponseReturn(
    TUSR_RcvParsedEvent OnRcvParsed);
```

```cpp
typedef void(_stdcall *TUSR_RcvParsedEvent)(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);
typedef boolean(_stdcall *FN_USR_OnRcvParsedOptionResponseReturn)(
    TUSR_RcvParsedEvent OnRcvParsed);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于接收数据点操作应答  }
procedure RcvParsedOptionResponseReturn_CBF(MessageID: LongInt;
  DevId, JsonStr: PWideChar);
var
  vsHint : string;
begin
  vsHint := Format(
    '【设备数据点操作应答事件】' + Chr(13) + Chr(10) +
    'MessageID:%d' + Chr(13) + Chr(10) +
    '设备ID:%s' + Chr(13) + Chr(10) +
    'JSON数据:%s',
    [MessageID,
     WideCharToString(DevId),
     WideCharToString(JsonStr)]);

  Writeln(vsHint);
end;
```

```pascal
{ 注册回调函数 }
USR_OnRcvParsedOptionResponseReturn(RcvParsedOptionResponseReturn_CBF);
```

```csharp
/* 自定义回调函数,用于接收数据点操作应答 */
private void RcvParsedOptionResponseReturn_CBF(
    int messageID, IntPtr DevId, IntPtr JsonStr)
{
    string sDevId = Marshal.PtrToStringAuto(DevId);
    string sJsonStr = Marshal.PtrToStringAuto(JsonStr);
    Log("【设备数据点操作应答事件】");
    Log("设备ID   : " + sDevId);
    Log("MsgId    : " + messageID.ToString());
    Log("JSON数据: " + sJsonStr);
}
```

```csharp
TUSR_RcvParsedEvent FRcvParsedOptionResponseReturn_CBF;
FRcvParsedOptionResponseReturn_CBF = new TUSR_RcvParsedEvent(
    RcvParsedOptionResponseReturn_CBF);
/* 注册回调函数 */
USR_OnRcvParsedOptionResponseReturn(FRcvParsedOptionResponseReturn_CBF);
```

```cpp
/* 自定义回调函数,用于接收数据点操作应答 */
static void _stdcall RcvParsedOptionResponseReturn_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr);

void CUsrCloudDllDemoDlg::RcvParsedOptionResponseReturn_CBF(
    long MessageID, LPCWSTR DevId, LPCWSTR JsonStr)
{
	CString str;
	str.Format(
        _T("【接收设备数据点操作应答事件】\n MessageID：%d\n DevId：%s\n JsonStr：%s\n"),
         MessageID, DevId, JsonStr);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */

FN_USR_OnRcvParsedOptionResponseReturn USR_OnRcvParsedOptionResponseReturn;  
USR_OnRcvParsedOptionResponseReturn = (FN_USR_OnRcvParsedOptionResponseReturn)GetProcAddress(
    hUsrCloud, "USR_OnRcvParsedOptionResponseReturn");
USR_OnRcvParsedOptionResponseReturn(RcvParsedOptionResponseReturn_CBF);
```

> 数据点操作应答 JSON数据格式

    {
        "optionResponse": [
            {
                "result":"1", //1 操作成功 0 代表不成功
                "dataId":"100",//数据点,如果是透传那么该字段为空
                "option":"1" //1.数据处于待发送状态 2.数据已发送
            }
        ],
        "devName":"123"
    }

<br>

**函数原型:<br><br>`boolean USR_OnRcvParsedOptionResponseReturn(TUSR_RcvParsedEvent OnRcvParsed);`**

参数|描述
----|----
OnRcvParsed |[in] 接收设备数据点操作应答回调函数   [TUSR_RcvParsedEvent 定义](#Define_TUSR_RcvParsedEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

#### <span id = "Define_TUSR_RcvParsedEvent"></span>
**TUSR_RcvParsedEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_RcvParsedEvent )(
                long MessageID,
                LPCWSTR DevId,
                LPCWSTR JsonStr
                );`**

参数|描述
----|----
MessageID |[out] 消息ID。
DevId|[out] 设备ID,消息来源
JsonStr|[out] JSON字符串

### --------- 云交换机操作 ---------

### <aside>USR_OnRcvRawFromDev 设置 接收设备原始数据流回调函数</aside>

> USR_OnRcvRawFromDev 设置 接收设备原始数据流回调函数 声明:

```pascal
TUSR_RcvRawFromDevEvent = procedure(
    MessageID: LongInt; DevId: PWideChar;  
    pData: PByte; DataLen: Integer); stdcall;
```

```pascal
function USR_OnRcvRawFromDev(
    OnRcvRawFromDev: TUSR_RcvRawFromDevEvent): Boolean; 
    stdcall; external 'UsrCloud.dll';
```

```csharp
public delegate void TUSR_RcvRawFromDevEvent(int MessageID, 
    IntPtr DevId, IntPtr pData, int DataLen);
```

```csharp
[DllImport("UsrCloud.dll", CharSet = CharSet.Auto, 
    EntryPoint = "USR_OnRcvRawFromDev", 
    CallingConvention = CallingConvention.StdCall)]
public static extern bool USR_OnRcvRawFromDev(
    TUSR_RcvRawFromDevEvent OnRcvRawFromDev);
```

```cpp
typedef void(_stdcall *TUSR_RcvRawEvent)(
    long MessageID, LPCWSTR DevId, void *pData, long DataLen);
typedef boolean(_stdcall *FN_USR_OnRcvRawFromDev)(
    TUSR_RcvRawEvent OnRcvRaw);
```

> 调用,一般在USR_Init执行成功之后调用 

```pascal
{ 自定义回调函数,用于接收设备原始数据流  }
procedure RcvRawFromDev_CBF(MessageID: LongInt; DevId: PWideChar;
  pData: PByte; DataLen: Integer);
var
  vsHint, vsHexData : string;
begin
  vsHexData := Pbuf2HexStr(PByteArray(pData), DataLen);
  vsHint := Format(
    '【接收数据流事件】' + Chr(13) + Chr(10) +
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
USR_OnRcvRawFromDev(RcvRawFromDev_CBF);
```

```csharp
/* 自定义回调函数,用于接收设备原始数据流 */
private void RcvRawFromDev_CBF(
    int messageID, IntPtr devId, IntPtr pData, int DataLen)
{
    string sDevId = Marshal.PtrToStringAuto(devId);
    byte[] byteArr = new byte[DataLen];
    Marshal.Copy(pData, byteArr, 0, DataLen);
    string sHex = BitConverter.ToString(byteArr).Replace(
        "-", " ");
    Log("【接收数据流事件】");
    Log("设备ID   : " + sDevId);
    Log("MsgId    : " + messageID.ToString());
    Log("接收数据(Hex): " + sHex);
}
```

```csharp
TUSR_RcvRawFromDevEvent FRcvRawFromDev_CBF;
FRcvRawFromDev_CBF = new TUSR_RcvRawFromDevEvent(
    RcvRawFromDev_CBF);
/* 注册回调函数 */
USR_OnRcvRawFromDev(FRcvRawFromDev_CBF);
```

```cpp
/* 自定义回调函数,用于接收设备原始数据流 */
static void _stdcall RcvRawFromDev_CBF(
    long MessageID, LPCWSTR DevId, void *pData, long DataLen);

void CUsrCloudDllDemoDlg::RcvRawFromDev_CBF(
    long MessageID, LPCWSTR DevId, void *pData, long DataLen)
{
	CString sHex, sByte;
	byte* pBuf = (byte*)pData;
	for (int i = 0; i < DataLen; i++)
	{
		sByte.Format(_T("%02x "), pBuf[i]);
		sHex += sByte;
	}
	
	CString str;
	str.Format(
        _T("【接收设备原始数据流事件】\n MessageID：%d\n DevId：%s\n 内容(HEX)：%s\n"), 
        MessageID, DevId, sHex);
	((CUsrCloudDllDemoDlg*)theApp.GetMainWnd())->AppendLog(str);
}

/* 注册回调函数 */
FN_USR_OnRcvRawFromDev USR_OnRcvRawFromDev;  
USR_OnRcvRawFromDev = (FN_USR_OnRcvRawFromDev)GetProcAddress(
    hUsrCloud, "USR_OnRcvRawFromDev");
USR_OnRcvRawFromDev(RcvRawFromDev_CBF);
```

<br>

**函数原型:<br><br>`boolean USR_OnRcvRawFromDev(TUSR_RcvRawFromDevEvent OnRcvRawFromDev);`**

参数|描述
----|----
OnRcvRawFromDev |[in] 接收数据回调函数   [TUSR_RcvRawFromDevEvent 定义](#Define_TUSR_RcvRawFromDevEvent)

返回值| 描述
---- | ----
boolean| 成功返回 true ,失败返回 false

<span id = "Define_TUSR_RcvRawFromDevEvent"></span>
**TUSR_RcvRawFromDevEvent 定义**

<br>

**`typedef void(__stdcall *TUSR_RcvRawFromDevEvent )(
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
1.0.0 | 2017-7-26 | 初版 | 张振鸣
<a href='old_1.0.1'><b>1.0.1</b></a> | 2017-8-8 | 修改USR_Publish一处提示| 张振鸣
2.1.0 | 2017-12-07 | 1. 增加json格式数据协议<br>2. 增加相关函数,以解决子账号设备消息的订阅推送问题| 张振鸣