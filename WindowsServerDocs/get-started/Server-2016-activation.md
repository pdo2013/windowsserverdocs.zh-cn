---
title: Microsoft 服务器激活
description: 如何激活 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 09/19/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a45d5cc7a54
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 79f15c4c9c635138ae74a61fe1c259b97717155e
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150847"
---
# Windows Server 2016 激活

以下信息概括介绍使用涉及 Windows Server 2016 的密钥管理服务 (KMS) 激活时需要关注的初步规划注意事项。 有关所涉及操作系统早于此处所列操作系统的 KMS 激活的信息，请参阅[步骤 1：查看并选择激活方法](https://technet.microsoft.com/library/jj134256(WS.11).aspx)。

KMS 使用客户端 - 服务器模式激活客户端。 KMS 客户端连接到一台 KMS 服务器（称为 KMS 主机）进行激活。 KMS 主机必须位于本地网络。

KMS 主机不必是专用服务器，KMS 可与其他服务共用一台主机。 你可以在运行 Windows 10、Windows Server 2016、Windows Server 2012 R2、Windows 8.1 或 Windows Server 2012 的任何物理或虚拟系统上运行 KMS 主机。

在 Windows10 或 Windows 8.1 上运行的 KMS 主机只能激活运行客户端操作系统的计算机。
下表总结了 KMS 主机和客户端（包括 Windows Server 2016 和 Windows 10 客户端）对网络的要求。

>[!NOTE]
>**注意：** 在 KMS 服务器上可能需要更新为支持的任何这些更高版本的客户端的激活。 如果收到激活错误，请检查你是否具有在此表下面列出的相应更新。

|产品密钥组|KMS 可以托管在上|由此 KMS 主机激活的 Windows 版本|  
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|  
|适用于 Windows Server 2016 的批量许可证|WindowsServer 2012<br /><br />Windows Server 2012 R2<br /><br />Windows Server 2016<br /><br />|Windows Server 半年频道 <br><br>Windows Server 2016（所有版本）<br /><br />Windows 10 LTSB（2015 和 2016）<br /><br />Windows 10 专业版<br /><br />Windows 10 企业版<br /><br />Windows 10 专业工作站版<br><br>Windows 10 教育版<br><br>Windows Server 2012 R2（所有版本）<br /><br />Windows8.1 专业版<br /><br />Windows 8.1 企业版<br /><br />Windows Server 2012（所有版本）<br /><br />Windows Server 2008 R2 （所有版本）<br /><br />Windows Server 2008 （所有版本）<br /><br />Windows7 专业版<br /><br />Windows7 企业版| 
|适用于 Windows 10 的批量许可证|Windows 7<br /><br />Windows 8.1<br /><br /> Windows 10|Windows 10 专业版<br /><br /> Windows 10 专业版 N<br /><br /> Windows 10 企业版<br /><br /> Windows 10 企业版 N<br /><br /> Windows 10 教育版<br /><br /> Windows 10 教育版 N<br /><br /> Windows 10 企业版 LTSB (2015)<br /><br /> Windows 10 企业版 LTSB N (2015)<br /><br /> Windows 10 专业工作站版<br><br>Windows8.1 专业版<br /><br /> Windows 8.1 企业版<br /><br /> Windows7 专业版<br /><br /> Windows7 企业版<br /><br />|  
|“适用于 Windows 10 的 Windows Server 2012 R2”批量许可证|WindowsServer 2008 R2<br /><br /> Windows Server 2012 Standard<br /><br /> Windows Server 2012 Datacenter<br /><br /> Windows Server 2012 R2 Standard<br /><br />Windows Server 2012 R2 Datacenter|Windows 10 专业版<br /><br /> Windows 10 企业版<br /><br />Windows 10 企业版 LTSB (2015)<br><br>Windows 10 专业工作站版<br><br>Windows 10 教育版<br><br> Windows Server 2012 R2（所有版本）<br /><br /> Windows8.1 专业版<br /><br /> Windows 8.1 企业版<br /><br /> Windows Server 2012（所有版本）<br /><br /> Windows Server 2008 R2 （所有版本）<br /><br />Windows Server 2008 （所有版本）<br /><br /> Windows7 专业版<br /><br /> Windows7 企业版|

> [!NOTE]  
>你可能需要安装这些更新中的一个或多个，具体取决于 KMS 服务器运行的操作系统以及你想要激活的操作系统。
>- Windows 7 或 Windows Server 2008 R2 上的 KMS 安装必须经过更新才可支持对运行 Windows 10 的客户端进行激活。 有关详细信息，请参阅[，使 Windows 7 和 Windows Server 2008 R2 KMS 主机能够激活 Windows 10 的更新](https://support.microsoft.com/help/3079821/update-that-enables-windows-7-and-windows-server-2008-r2-kms-hosts-to-activate-windows-10)。  
>- Windows Server 2012 上的 KMS 安装必须经过更新才可支持对运行 Windows 10 和 Windows Server 2016 或更高版本客户端或服务器操作系统的客户端进行激活。 有关详细信息，请参阅[2016 年 7 月更新汇总为 Windows Server 2012](https://support.microsoft.com/help/3172615/july-2016-update-rollup-for-windows-server-2012)。 
>- Windows 8.1 或 Windows Server 2012 R2 上的 KMS 安装必须经过更新才可支持对运行 Windows 10 和 Windows Server 2016 或更高版本客户端或服务器操作系统的客户端进行激活。 有关详细信息，请参阅[2016 年 7 月更新汇总为 Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8.1-and-windows-server-2012-r2)。  
>- 无法更新 Windows Server 2008 R2 来支持对运行 Windows Server 2016 或更高版本操作系统的客户端进行激活。 

一台 KMS 主机能够支持无限数量的 KMS 客户端。 如果客户端数量超过 50 个，我们建议至少准备两台 KMS 主机，以防某一台 KMS 主机不可用。 大多数组织单位运行两台 KMS 主机可以满足整个基础结构的需求。

# 满足 KMS 的操作要求
KMS 能够激活物理和虚拟计算机，但是要使用 KMS 激活，网络中计算机的数量必须达到最低要求（称为激活阈值）。 KMS 客户端只有在达到此阈值之后才会激活。 为确保达到激活阈值，KMS 主机会计算网络中请求激活的计算机的数量。

KMS 主机计算最近的连接数。 当客户端或服务器联系 KMS 主机时，主机将计算机 ID 添加到其计数，然后在其响应中返回当前的计数值。 计数足够高时将激活客户端或服务器。 计数为 25 或更高时将激活客户端。 计数为 5 或更大时，将激活服务器和批量版 Microsoft Office 产品。 KMS 只对过去 30 天内的唯一连接计数，且仅存储 50 个最新联系人。

KMS 激活的有效期为 180 天，这一时期称为激活有效期间隔。 要保持激活状态，KMS 客户端至少要每 180 天连接一次 KMS 主机，以续订它们的激活。 默认情况下，KMS 客户端计算机会每 7 天尝试一次激活续订。 客户端的激活续订之后，激活有效期将重新开始计算。

# 满足 KMS 功能要求

KMS 激活需要 TCP/IP 连接。 KMS 主机和客户端默认配置为使用域名系统 (DNS)。 默认情况下，KMS 主机使用 DNS 动态更新来自动发布 KMS 客户端查找并连接主机所需的信息。 你可以接受这些默认设置；如果有特殊的网络和安全配置要求，则可手动配置 KMS 主机和客户端。

第一个 KMS 主机激活之后，第一个主机上使用的 KMS 密钥最多可用来激活网络上的另外 5 台 KMS 主机。 KMS 主机激活之后，管理员最多可使用同一密钥将同一台主机重新激活 9 次。

如果你的组织单位需要 6 台以上的 KMS 主机，则应为组织单位的 KMS 密钥请求更多的激活次数 － 例如，如果一份批量许可协议涵盖 10 个物理位置，并且你希望每个位置有一台本地的 KMS 主机。

>[!NOTE] 
>若要解决此特殊情况，请联系你的激活呼叫中心。 有关详细信息，请参阅 [Microsoft 批量许可]( https://www.microsoft.com/licensing)。

默认情况下，运行批量许可版本的 Windows 10、Windows Server 2016、Windows 8.1、Windows Server 2012 R2、Windows Server 2012、Windows 7、Windows Server 2008 R2 的计算机为无需额外配置的 KMS 客户端。

如果要将计算机从 KMS 主机、MAK 或零售版 Windows 转换为 KMS 客户端，请安装适用的 KMS 客户端安装密钥。 有关详细信息，请参阅[KMS 客户端安装密钥](KMSclientkeys.md)。 
