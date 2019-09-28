---
title: atmadm
description: 适用于**atmadm**的 Windows 命令主题-监视异步传输模式（atm）网络上的 AtM 呼叫管理器注册的连接和地址。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fdb0070063f2bfc6be7b762d71e2f30216f9d8d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382626"
---
# <a name="atmadm"></a>atmadm

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

监视由 atM 呼叫管理器在异步传输模式（atM）网络上注册的连接和地址。 可以使用**atmadm**显示 atM 适配器上的传入和传出呼叫的统计信息。 使用不带参数的**atmadm**显示监视活动的 atM 连接状态的统计信息。 

## <a name="syntax"></a>语法

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|-------|--------|
|/c|显示与此计算机上安装的 atM 网络适配器的所有当前连接的调用信息。|
|/a|显示在此计算机中安装的每个适配器的已注册 atM 网络服务访问点（NSAP）地址。|
|/s|显示用于监视活动的 atM 连接状态的统计信息。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

- **Atmadm/c**命令生成类似于以下内容的输出：

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
    
    下表包含**atmadm/c**示例输出中每个元素的说明。
    
    |数据类型|屏幕显示|描述|
    |--------|---------|--------|
    |连接信息|输入/输出|调用的方向。  **在中**，从另一台设备到 atM 网络适配器。  **Out**是从 atM 网络适配器到另一台设备。|
    ||PMP|点到 multipoint 调用。|
    ||P-P|点到点调用。|
    ||SVC|连接位于交换式虚拟线路上。|
    ||PVC|连接位于永久虚拟线路上。|
    |VPI/VCI 信息|VPI/VCI|传入或传出调用的虚拟路径和虚拟通道。|
    |远程地址/媒体参数|47000580FFE1000000F21A2E180000C110081500|调用 **（在中）** 或调用 **（Out）** atM 设备的 NSAP 地址。|
    ||**Tx**|**Tx**参数包含以下三个元素：<br /><br />-默认或指定的位速率类型（UBR、CBR、VBR 或 ABR）<br />-默认或指定的线路速度<br />-指定的服务数据单元（SDU）大小|
    ||**Rx**|**Rx**参数包含以下三个元素：<br /><br />-默认或指定的位速率类型（UBR、CBR、VBR 或 ABR）<br />-默认或指定的线路速度<br />-指定的 SDU 大小|
    
- **Atmadm/a**命令生成类似于以下内容的输出：

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- **Atmadm/s**命令生成类似于以下内容的输出：

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
    
    下表包含**atmadm/s**示例输出中每个元素的说明。
    
    |调用管理器统计信息|描述|
    |-------------|--------|
    |当前活动的调用|此计算机上安装的 atM 适配器上当前处于活动状态的呼叫。|
    |成功的传入呼叫总数|已成功从该 atM 网络上的其他设备接收调用。|
    |成功的传出呼叫总数|从此计算机上的网络中的其他 atM 设备调用已成功完成。|
    |不成功的传入调用|无法连接到此计算机的传入呼叫。|
    |不成功的传出呼叫|未能连接到网络上的其他设备的传出呼叫。|
    |远程关闭的调用|网络上的远程设备关闭的呼叫。|
    |调用在本地关闭|此计算机关闭的调用。|
    |发送的信号和 ILMI 数据包|发送到此计算机尝试连接到的交换机的集成本地管理接口（ILMI）数据包的数量。|
    |收到的信号和 ILMI 数据包|从 atM 交换机接收的 ILMI 数据包的数量。|
    
## <a name="BKMK_Examples"></a>示例

若要显示此计算机上安装的 atM 网络适配器的所有当前连接的呼叫信息，请键入：

```
atmadm /c
```

若要为此计算机上安装的每个适配器显示已注册的 atM 网络服务访问点（NSAP）地址，请键入：

```
atmadm /a
```

若要显示监视活动的 atM 连接状态的统计信息，请键入：

```
atmadm /s
```

## <a name="additional-references"></a>其他参考

- [命令行语法项](command-line-syntax-key.md)
