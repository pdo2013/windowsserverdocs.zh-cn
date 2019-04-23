---
title: mode
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde0e9786f72823f446202f1c87ad8e9e181d29c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848198"
---
# <a name="mode"></a>mode



显示系统状态、 更改系统设置，或重新配置端口或设备。 如果使用不带参数，**模式下**显示控制台和可用的 COM 设备的所有可控制的属性。

可以使用**模式下**执行以下任务，每个任务使用不同的语法：
-   [若要配置的串行通讯端口](#BKMK_1)
-   [若要显示的所有设备或单个设备的状态](#BKMK_2)
-   [若要重定向到串行通讯端口输出并行端口](#BKMK_3)
-   [若要选择、 刷新或显示在控制台的代码页号](#BKMK_4)
-   [若要更改的命令提示符下屏幕缓冲区大小](#BKMK_5)
-   [若要设置键盘字符重复率](#BKMK_6)

## <a name="BKMK_1"></a>若要配置的串行通讯端口

### <a name="syntax"></a>语法

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|Com\<M > [::]|指定异步 Prncnfg.vbshronous 通信端口数。|
|baud=\<B>|以每秒位数为指定的传输速率。 下表列出了有效的州名的缩写*B*和其相关的费率。</br>-   **11** = 110 波特率</br>-   **15** = 150 波特率</br>-   **30** = 300 波特率</br>-   **60** = 600 波特率</br>-   **12** = 1200 波特率</br>-   **24** = 2400 波特率</br>-   **48** = 4800 波特</br>-   **96** = 9600 波特</br>-   **19** = 19200 波特率|
|parity=\<P>|指定系统如何使用奇偶校验位来检查传输错误。 下表列出了有效值*P*。默认值是**e**。 并非所有计算机都支持的值**m**并**s**。</br>-   **n** = 无</br>-   **e** = even</br>-   **o** = odd</br>-   **m** = 标记</br>-   **s** = 空格|
|data=\<D>|指定的字符中的数据比特数。 有效值**d**位于范围 5 至 8。 默认值为 7。 并非所有计算机都支持 5 和 6 的值。|
|stop=\<S>|指定的定义字符的终点的停止位的数目：1、 1.5 或 2。 如果波特率 110，默认值为 2。 否则，默认值为 1。 并非所有计算机都支持 1.5 的值。|
|to={on | 关闭}|指定无限期超时处理是开还是关。 默认处于关闭状态。|
|xon={on | 关闭}|指定数据的流控制的 xon 或 xoff 协议是开还是关。|
|odsr={on | 关闭}|指定使用该数据设置就绪 (DSR) 线路输出握手是开还是关。|
|octs={on | 关闭}|指定使用清除发送 (CTS) 线路输出握手是开还是关。|
|dtr={on | off | hs}|指定数据终端就绪 (DTR) 线路是启用还是禁用还是设置为握手。|
|rts={on | off | hs | tg}|指定是否请求发送 (RTS) 线路设置为 on、 off、 握手或切换。|
|idsr={on | 关闭}|指定 DSR 线路敏感度是开还是关。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_2"></a>若要显示的所有设备或单个设备的状态

### <a name="syntax"></a>语法

```
mode [<Device>] [/status]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<设备 >|指定你想要显示的状态的设备的名称。|
|/status|请求的任何重定向的并行打印机的状态。 你可以将缩写 **/status**作为命令行选项 **/sta**。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

如果使用不带参数，**模式下**显示在系统安装的所有设备的状态。

## <a name="BKMK_3"></a>若要重定向到串行通讯端口输出并行端口

### <a name="syntax"></a>语法

```
mode lpt<N>[:]=com<M>[:]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|lpt\<N > [::]|必需。 指定的并行端口。 有效值*N*范围是 1 到 3。|
|com\<M > [::]|必需。 指定的串行端口。 有效值*M* 1 到 4 的范围内。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

必须是 Administrators 组的成员才能重定向打印。

### <a name="examples"></a>示例

若要设置您的系统，以便它将并行打印机的输出发送到串行打印机，必须使用**模式下**命令两次。 第一次，使用**模式下**可配置的串行端口。 第二个时，请**模式下**并行打印机输出重定向到在第一个指定的串行端口**模式**命令。

例如，如果串行打印机工作速率为 4800 波特带有奇偶，并且连接到 COM1 端口 （您的计算机上的第一个串行连接），请键入：
```
mode com1 48,e,,,b
mode lpt1=com1
```
打印文件之前，如果您将重定向并行打印机输出从 LPT1 到 COM1，但然后决定你想要通过使用 LPT1 打印文件，键入以下命令：
```
mode lpt1
```
此命令可阻止重定向到 COM1 从 LPT1 文件。

## <a name="BKMK_4"></a>若要选择、 刷新或显示在控制台的代码页号

### <a name="syntax"></a>语法

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<设备 >|必需。 指定你想要选择的代码页的设备。 CON 是设备的唯一有效名称。|
|代码页选择 =|必需。 指定要用于指定设备的代码页。 你可以将缩写**代码页****选择**作为**cp** **sel**。|
|\<YYY>|必需。 指定要选择的代码页的数目。 下表显示每个代码页支持其国家/地区或语言。</br>437:美国</br>850:多语言 (拉丁文我)</br>852:西里尔语 （俄语）</br>855:西里尔语 （俄语）</br>857:土耳其语</br>860:葡萄牙语</br>861:冰岛语</br>863:加拿大法语</br>865:北欧</br>866:俄语</br>869:现代希腊语|
|codepage|必需。 显示的数字代码页 （如果有） 为指定的设备选择。|
|/status|显示指定设备为选择的当前代码页号。 你可以将缩写 **/status**到 **/sta**。 该值指示是否指定 **/status**，**模式下的代码页**显示为指定的设备选择的代码页的数字。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_5"></a>若要更改的命令提示符下屏幕缓冲区大小

### <a name="syntax"></a>语法

```
mode con[:] [cols=<C>] [lines=<N>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|con[:]|必需。 指示更改适用于在命令提示符窗口。|
|cols=\<C>|在命令提示符下屏幕缓冲区中指定列的数。|
|lines=\<N>|在命令提示符下屏幕缓冲区中指定行的数。|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_6"></a>若要设置键盘字符重复率

### <a name="syntax"></a>语法

```
mode con[:] [rate=<R> delay=<D>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|con[:]|必需。 是指键盘。|
|rate=\<R>|当您按住某个键时在屏幕上指定的重复字符的速率。|
|delay=\<D>|指定按和之前的字符输出重复按下某键后经过的时间量。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注

-   字符重复率是按下该字符的密钥，当某字符重复的速率。 字符重复率有两个组件、 速率和延迟。 某些键盘无法识别此命令。
-   使用 **速率 = * * * R*

    有效值范围从 1 到 32。 这些值是等于每秒大约 2 到 30 字符。 默认值为为 IBM AT 兼容键盘、 20 和 21 的 IBM PS/2-兼容键盘。 如果设置的速率，则还必须设置延迟。
-   使用**延迟**=*D*

    有效值*D*是 1、 2、 3 和 4 （代表 0.25、 0.50、 0.75 和 1 秒）。 默认值为 2。 如果设置延迟，则还必须设置速率。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)