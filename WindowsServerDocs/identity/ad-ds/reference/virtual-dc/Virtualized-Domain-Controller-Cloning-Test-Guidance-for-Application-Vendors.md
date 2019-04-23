---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: 适用于应用程序供应商的虚拟化域控制器克隆测试指南
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b2303bc837cdaf9f6e7ebd4b3ccbf6c66aa7ad2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879338"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>适用于应用程序供应商的虚拟化域控制器克隆测试指南

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题说明什么应用程序供应商应考虑以帮助确保其应用程序将继续正常运行虚拟化的域控制器 (DC) 克隆过程完成后。 它介绍了克隆过程的那些方面，该目标应用程序供应商和可能有必要进一步的测试的方案。 应用程序供应商验证了其应用程序适用于已克隆的虚拟化的域控制器，建议列表中的社区内容的链接以及本主题底部的应用程序的名称的应用组织的网站，用户可以从何处了解有关验证的详细信息。  
  
## <a name="overview-of-virtualized-dc-cloning"></a>虚拟化 DC 克隆的概述  
虚拟化的域控制器克隆过程中将详细介绍[Active Directory 域服务 (AD DS) 虚拟化 (级别 100) 简介](https://technet.microsoft.com/library/hh831734.aspx)和[虚拟化域控制器技术参考 (级别 300)](https://technet.microsoft.com/library/jj574214.aspx)。 从应用程序供应商的角度来看，这些是评估的克隆到你的应用程序的影响时需要考虑一些注意事项：  
  
-   原始计算机不会被销毁。 它将保留在网络上，与客户端交互。 与重命名其中删除原始计算机的 DNS 记录，不同的源域控制器的原始记录保留。  
  
-   在克隆过程中，新计算机最初运行旧的计算机的标识下的时间在短时间内直到克隆过程就会启动并进行必要的更改。 创建记录有关主机的应用程序应确保克隆的计算机不会覆盖在克隆过程中记录有关原始主机。  
  
-   克隆是唯一的虚拟化的域控制器未一个常规用途扩展，用于克隆其他服务器角色特定的部署功能。 某些服务器角色是专门不支持克隆：  
  
    -   动态主机配置协议 (DHCP)  
  
    -   Active Directory 证书服务 (AD CS)  
  
    -   Active Directory 轻型目录服务 (AD LDS)  
  
-   克隆过程的一部分，复制整个表示原始 DC 的 VM，因此也会复制该 VM 上的任何应用程序状态。 验证应用程序适应这一更改在克隆的 DC 上的本地主机的状态或如果任何干预是必需的例如服务重新启动。  
  
-   克隆的一部分，新 DC 获取新的计算机标识和预配本身作为副本 DC 中的拓扑。 验证是否在应用程序依赖于计算机标识，如其名称、 帐户、 SID，等等。 没有它会自动调整为在克隆上的计算机标识的更改？ 如果该应用程序缓存的数据，请确保它不依赖于可缓存的计算机标识数据。  
  
## <a name="what-is-interesting-for-application-vendors"></a>有趣的应用程序供应商是什么？  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
运行应用程序或服务的域控制器不能克隆直到应用程序或服务是：  
  
-   通过使用 Get-ADDCCloningExcludedApplicationList Windows PowerShell cmdlet 添加到 CustomDCCloneAllowList.xml 文件  
  
-或者-  
  
-   从域控制器中删除  
  
第一次用户在运行 Get-addccloningexcludedapplicationlist cmdlet，它返回一系列服务和应用程序在域控制器上运行，但不在服务和支持克隆的应用程序的默认列表中。 默认情况下，你的服务或应用程序将不会列出。 若要添加你的服务或应用程序到列表的应用程序和服务可以安全地克隆，用户运行 Get-addccloningexcludedapplicationlist cmdlet 再次使用-GenerateXML 选项以将其添加到 CustomDCCloneAllowList.xml 文件。 有关详细信息，请参阅[步骤 2:运行 Get-addccloningexcludedapplicationlist cmdlet](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet)。  
  
### <a name="distributed-system-interactions"></a>分布式的系统之间的交互  
通常独立于本地计算机的服务通过或失败时参与克隆。 分布式的服务需要关心如何同时在主计算机的两个实例在网络上的时间在短时间内。 这可能表现为尝试提取来自合作伙伴系统的信息在克隆作为新的供应商的标识已注册的其中一个服务实例。 或在同一时间使用不同的结果，该服务的两个实例可能会将信息推送到 AD DS 数据库。 例如，它是不确定哪台计算机将进行通信时有 Windows 测试技术 (WTT) 服务的两台计算机位于域控制器的网络。  
  
为分布式的 DNS 服务器服务，克隆过程小心地避免克隆域控制器开始与新的 IP 地址时覆盖源域控制器的 DNS 记录。  
  
您不应依赖的克隆结束前删除所有旧标识的计算机上。 新的域控制器被提升新的上下文中后，选择的 Sysprep 提供程序运行清理的计算机的其他状态。 例如，它是计算机的现在将删除旧的证书和计算机可访问的加密机密会更改。  
  
克隆的时间各不相同的最大因素是有多少个对象都将从 PDC 复制。 更早版本媒体会增加完成克隆所需的时间。  
  
你的服务或应用程序是未知的因为它将保持运行。 克隆过程不会更改非 Windows 服务的状态。  
  
此外，新的计算机具有与原始计算机不同的 IP 地址。 这些行为可能会导致你的服务或应用程序，具体取决于服务或应用程序在此环境中的行为方式的负面影响。  
  
## <a name="additional-scenarios-suggested-for-testing"></a>建议用于测试的其他方案  
  
### <a name="cloning-failure"></a>克隆失败  
服务供应商应该测试此方案中，因为克隆失败时在计算机启动到目录服务修复模式 (DSRM)，一种安全模式。 此时计算机尚未完成克隆。 某些状态可能已更改，而且某些状态可能仍存在从原始域控制器。 测试这种情况下，若要了解它可以对你的应用程序产生的影响。  
  
若要引入克隆失败，尝试克隆而无需授予其权限要克隆的域控制器。 在这种情况下，计算机将仅更改了 IP 地址并且仍有其状态从原始域控制器的大多数。 有关授予克隆域控制器权限的详细信息，请参阅[步骤 1:授予源虚拟化的域控制器克隆权限](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source)。  
  
### <a name="pdc-emulator-cloning"></a>克隆的 PDC 仿真器  
服务和应用程序供应商应测试此方案，因为没有重启系统时克隆 PDC 仿真器。 此外，以允许新的克隆，以便与 PDC 仿真器在克隆过程中以临时身份执行大多数克隆。  
  
### <a name="writable-versus-read-only-domain-controllers"></a>只读域控制器与可写  
服务和应用程序供应商应使用相同类型的域控制器克隆测试 (即，在可写或只读域控制器上) 服务计划上运行。  
  


