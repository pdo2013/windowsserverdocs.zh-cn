---
title: 连接时用户收到“远程桌面服务当前繁忙”消息
description: 排查当用户启动远程桌面连接时出现“远程桌面服务当前繁忙”错误的问题。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 889fd83925081ac1dce386b1cd18fbef59586eb5
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529897"
---
# <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>连接时用户收到“远程桌面服务当前繁忙”消息

若要确定此问题的相应对策，请查看以下问题：

- “远程桌面服务”服务是否无响应（例如，远程桌面客户端在“欢迎”屏幕上呈“挂起”状态）。  
   - 如果该服务无响应，请参阅 [RDSH 服务器内存问题](#rdsh-server-memory-issue)。
   - 如果客户端似乎能够正常地与该服务交互，请继续执行下一步骤。
- 如果一个或多个用户断开其远程桌面会话的连接，这些用户是否可以再次连接？  
   - 如果不管有多少个用户断开连接其会话，该服务仍会不断地拒绝连接，请参阅 [RD 侦听器问题](#rd-listener-issue)。
   - 如果在一些用户断开连接其会话后，该服务再次开始接受连接，请[检查连接限制策略](#check-the-connection-limit-policy)。

## <a name="rdsh-server-memory-issue"></a>RDSH 服务器内存问题

在某些 Windows Server 2012 R2 RDSH 服务器上发现了内存泄漏。 一段时间后，这些服务器会开始拒绝远程桌面连接和本地控制台登录，并显示如下所示消息：

> 由于远程桌面服务当前繁忙，无法完成你要执行的任务。 请过几分钟重试。 其他用户应该仍可登录。

尝试进行连接的远程桌面客户端也变得无响应。

若要解决此问题，请重启 RDSH 服务器。

若要解决此问题，请向 RDSH 服务器应用 KB 4093114 [2018 年 4 月 10 日 - KB4093114（每月汇总）](https://support.microsoft.com/help/4093114/)。

## <a name="rd-listener-issue"></a>RD 侦听器问题

在直接从 Windows Server 2008 R2 升级到 Windows Server 2012 R2 或 Windows Server 2016 的某些 RDSH 服务器上注意到了一个问题。 当远程桌面客户端连接到 RDSH 服务器时，RDSH 服务器会为用户会话创建一个 RD 侦听器。 受影响的服务器将统计每次用户连接时都会递增，但永远不会递减的 RD 侦听器计数。

若要解决此问题，可使用以下方法：

  - 重启 RDSH 服务器，以重置 RD 侦听器的计数
  - 修改连接限制策略，将其设置为很大的值。 有关管理连接限制策略的详细信息，请参阅[检查连接限制策略](#check-the-connection-limit-policy)。

若要解决此问题，请将以下更新应用到 RDSH 服务器：

  - Windows Server 2012 R2：KB 4343891 [2018 年 8 月 30 日 - KB4343891（月度汇总预览）](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016：KB 4343884 [2018 年 8 月 30 日 - KB4343884（OS 内部版本 14393.2457）](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)

## <a name="check-the-connection-limit-policy"></a>检查连接限制策略

可以在单个计算机级别或者通过配置组策略对象 (GPO) 来设置同时建立的远程桌面连接数限制。 默认未设置该项限制。

若要检查当前设置并识别 RDSH 服务器上的任何现有 GPO，请以管理员身份打开命令提示符窗口，然后输入以下命令：
  
```cmd
gpresult /H c:\gpresult.html
```
   
完成此命令后，打开 **gpresult.html**。 在“计算机配置”\\“管理模板”\\“Windows 组件”\\“远程桌面服务”\\“远程桌面会话主机”\\“连接”中，找到“限制连接数”策略。  

  - 如果此策略的设置为“已禁用”，则表示组策略未限制 RDP 连接数。 
  - 如果此策略的设置为“已启用”，请检查“入选的 GPO”。   如果需要删除或更改连接限制，请编辑此 GPO。

若要强制实施策略更改，请在受影响的计算机上打开命令提示符窗口并输入以下命令：
  
```cmd
gpupdate /force
```
  
