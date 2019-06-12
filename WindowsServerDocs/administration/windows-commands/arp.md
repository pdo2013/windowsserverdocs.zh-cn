---
title: arp
description: Windows 命令主题**arp** -显示和修改使用存储 IP 地址和其解析的物理地址的地址解析协议 (arp) 缓存中的条目。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f7393c993601a5e1990cde3e6bc6763811062f4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435289"
---
# <a name="arp"></a>arp

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示和修改地址解析协议 (ARP) 缓存中的条目。 ARP 缓存中包含用来存储 IP 地址和其解析的以太网或令牌环的物理地址的一个或多个表。 没有为每个以太网或令牌环网络适配器在计算机上安装一个单独的表。 使用不带参数， **arp**显示帮助信息。
## <a name="syntax"></a>语法
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Parameters

|                参数                |                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                               |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /a [<Inetaddr>] [/n <ifaceaddr>]     | 显示当前的 arp 缓存表的所有接口。 /N 参数是区分大小写。<br /><br />若要显示特定的 IP 地址的 arp 缓存项，请使用**arp/a**与*Inetaddr*参数，其中*Inetaddr*是 IP 地址。 如果*Inetaddr*未指定，则使用第一个适用的接口。<br /><br />若要显示的特定接口的 arp 缓存表，请使用 * */n***ifaceaddr*参数结合 **/a**参数其中*ifaceaddr*是 IP 地址分配给的接口。 |
|    /g [<Inetaddr>] [/n <ifaceaddr>]     |                                                                                                                                                                                                                                                          与相同 **/a**。                                                                                                                                                                                                                                                           |
|      [/d <Inetaddr> [<ifaceaddr>]       |                                                                                           删除条目与特定的 IP 地址，其中*Inetaddr*是 IP 地址。<br /><br />若要删除特定接口的表中的条目，请使用*ifaceaddr*参数其中*ifaceaddr*是分配给的接口的 IP 地址。<br /><br />若要删除所有条目，请使用星号 (\*) 通配符来代替*Inetaddr*。                                                                                           |
| /s <Inetaddr> <Etheraddr> [<ifaceaddr>] |                                                                                                                     将静态条目添加到 IP 地址解析 arp 缓存*Inetaddr*到物理地址*Etheraddr*。<br /><br />若要将静态 arp 缓存条目添加到特定的接口的表，请使用*ifaceaddr*参数其中*ifaceaddr*是分配给的接口的 IP 地址。                                                                                                                     |
|                   /?                    |                                                                                                                                                                                                                                                  在命令提示符下显示帮助。                                                                                                                                                                                                                                                   |

## <a name="remarks"></a>备注
- 为 IP 地址*Inetaddr*并*ifaceaddr*以点分十进制表示法表示。
- 物理地址*Etheraddr*包含以十六进制表示法表示并由连字符 (例如，00-AA-00-4F-2A-9C) 分隔的 6 个字节。
- 通过添加的项 **/s**参数是静态的并且不对从 arp 缓存时间。 如果停止和启动 TCP/IP 协议，会删除条目。 若要创建永久静态 arp 缓存项，将适当**arp**一批中的命令文件，使用计划任务将在启动时运行的批处理文件。
  ## <a name="BKMK_Examples"></a>示例
  若要显示的所有接口的 arp 缓存表，请键入：
  ```
  arp /a
  ```
  若要显示接口分配 IP 地址为 10.0.0.99 的 arp 缓存表，请键入：
  ```
  arp /a /n 10.0.0.99
  ```
  若要添加静态 arp 缓存条目的 IP 地址 10.0.0.80 解析到 00-AA-00-4F-2A-9C 的物理地址，请键入：
  ```
  arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
  ```
  ## <a name="additional-references"></a>其他参考
- [命令行语法项](command-line-syntax-key.md)
