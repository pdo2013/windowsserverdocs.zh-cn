---
title: atmadm
description: Windows 命令主题**atmadm** -监视器连接和注册的 atM 地址调用管理器在异步传输模式 (atM) 网络。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a8a8883bad9aa2abdc90d5db5702ef42f46c8a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831768"
---
# <a name="atmadm"></a>atmadm

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

监视器连接和注册的 atM 地址调用异步传输模式 (atM) 网络上管理器。 可以使用**atmadm** atM 适配器上显示传入和传出调用的统计信息。 使用不带参数， **atmadm**显示监视 active atM 连接的状态的统计信息。 

## <a name="syntax"></a>语法

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|/c|显示调用的所有当前连接到此计算机上安装的 atM 网络适配器的信息。|
|/a|显示已注册的 atM 网络服务访问点 (NSAP) 地址为此计算机上安装的每个适配器。|
|/s|显示监视 active atM 连接的状态的统计信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- **Atmadm /c**命令生成的输出如下所示：

    ```
    Windows atM call Manager Statistics
    atM Connections on Interface : [009] Olicom atM PCI 155 Adapter
       Connection   VPI/VCI   remote address/
                              Media Parameters (rates in bytes/sec)
       In  PMP SVC    0/193   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/192   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  PMP SVC    0/191   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/190   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  P-P SVC    0/475   47000580FFE1000000F21A2E180000C110081501
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9188
       Out PMP SVC    0/194   47000580FFE1000000F21A2E180000C110081501 (0)
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9180
                              Rx:UBR,Peak 0,Avg 0,MaxSdu 0
       Out P-P SVC    0/474   4700918100000000613E5BFE010000C110081500
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
       In  PMP SVC    0/195   47000580FFE1000000F21A2E180000C110081500
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 0
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9180
    ```
    
    下表包含的每个元素中的说明**atmadm /c**示例输出。
    
    |数据类型|屏幕显示|描述|
    |--------|---------|--------|
    |连接信息|输入/输出|调用的方向。  **在**atM 网络适配器是从另一台设备。  **Out**从 atM 网络适配器为另一台设备。|
    ||PMP|多点到点调用。|
    ||P-P|点到点调用。|
    ||SVC|连接在交换虚拟线路。|
    ||PVC|连接是永久的虚拟线路上。|
    |VPI/VCI 信息|VPI/VCI|虚拟路径和虚拟通道的传入或传出调用。|
    |远程地址/媒体参数|47000580FFE1000000F21A2E180000C110081500|调用 NSAP 地址 **(In)** 或调用 **（缩小）** atM 设备。|
    ||**Tx**|**Tx**参数包含以下三个元素：<br /><br />-Default 或指定的比特率类型 （UBR，CBR、 VBR 或 ABR）<br />-Default 或指定的行的速度<br />-指定的服务数据单元 (SDU) 大小|
    ||**Rx**|**Rx**参数包含以下三个元素：<br /><br />-Default 或指定的比特率类型 （UBR，CBR、 VBR 或 ABR）<br />-Default 或指定的行的速度<br />-指定 SDU 大小|
    
- **Atmadm/a**命令生成的输出如下所示：

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- **Atmadm /s**命令生成的输出如下所示：

    ```
    Windows atM call Manager Statistics
    atM call Manager statistics for Interface : [009] Olicom atM PCI 155 Adapter
    Current active calls                        = 4
    Total successful Incoming calls             = 1332
    Total successful Outgoing calls             = 1297
    Unsuccessful Incoming calls                 = 1
    Unsuccessful Outgoing calls                 = 1
    calls Closed by remote                      = 1302
    calls Closed Locally                        = 1323
    Signaling and ILMI Packets Sent            = 33655
    Signaling and ILMI Packets Received        = 34989
    ```
    
    下表包含的每个元素中的说明**atmadm /s**示例输出。
    
    |调用管理器统计信息|描述|
    |-------------|--------|
    |当前活动的调用|在此计算机上安装的 atM 适配器调用当前处于活动状态。|
    |成功传入调用总数|已成功接收来自此 atM 网络上的其他设备的呼叫。|
    |总成功的传出呼叫|已成功完成到此网络上其他 atM 设备从这台计算机的调用。|
    |失败的传入呼叫|无法连接到此计算机的传入呼叫。|
    |失败的传出呼叫|无法连接到另一台设备在网络上的传出呼叫。|
    |通过远程调用已关闭|通过在网络上的远程设备关闭时调用。|
    |关闭本地调用|此计算机已关闭时调用。|
    |信号和 ILMI 发送的数据包数|发送到交换机的集成的本地管理接口 (ILMI) 数据包数的这台计算机正在尝试连接到。|
    |信号发送和接收的 ILMI 数据包|ILMI 收到的数据包数从 atM 交换机。|
    
## <a name="BKMK_Examples"></a>示例

若要显示此计算机上安装的 atM 网络适配器的所有当前连接的调用信息，请键入：

```
atmadm /c
```

若要显示此计算机上安装的每个适配器的已注册的 atM 网络服务访问点 (NSAP) 地址，请键入：

```
atmadm /a
```

若要显示监视 active atM 连接的状态的统计信息，请键入：

```
atmadm /s
```

## <a name="additional-references"></a>其他参考

- [命令行语法解答](command-line-syntax-key.md)
