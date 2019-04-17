---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: "复制测试应用程序供应商指南虚拟化的域控制器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 72c4e818f82d3252c45776b26fb59e095893f2c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>复制测试应用程序供应商指南虚拟化的域控制器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍哪些应用程序供应商应该考虑来帮助确保他们的应用程序将继续按虚拟化的域控制器 (DC) 复制过程完成后预期工作。 它涉及这些的克隆的过程的各个方面，该兴趣应用程序供应商，可能需要附加测试的方案。 有验证自己的应用程序适用有已复制的虚拟化的域控制器上的应用程序供应商被鼓励列表中的本主题，以及在用户可以详细了解验证你的组织的网站的链接底部的内容社区的应用程序的名称。  
  
## <a name="overview-of-virtualized-dc-cloning"></a>虚拟化 DC 复制概述  
复制过程虚拟化的域控制器中将详细介绍[简介 Active Directory 域服务 (广告 DS) 虚拟化 (级别 100)](https://technet.microsoft.com/library/hh831734.aspx)和[虚拟化域控制器技术参考 (级别 300)](https://technet.microsoft.com/library/jj574214.aspx)。 从应用程序供应商角度看，以下是一些注意事项考虑评估复制到你的应用程序的影响时：  
  
-   原始的计算机不被销毁。 该应用保留在网络上，与客户端交互。 与不同重命名移除 DNS 记录的原始计算机的位置，将保留源域控制器的原始记录。  
  
-   克隆过程新计算机最初运行小段时间在旧的计算机的身份直到克隆过程会启动并进行必要的更改。 创建有关主机记录的应用程序，应确保复制的计算机不会覆盖克隆过程中记录关于原始主机。  
  
-   复制是特定部署功能仅虚拟化的域控制器不常规用途扩展复制其他服务器角色。 复制专门不支持某些服务器的角色：  
  
    -   动态主机配置协议(DHCP)  
  
    -   Active Directory 证书服务（广告客户服务）  
  
    -   Active Directory 轻型目录服务 (广告 LDS)  
  
-   作为克隆的过程的一部分，复制整个 VM 表示原始直流，以便在该 VM 任何应用程序状态也被复制。 验证该应用程序适应状态的本地主机上的复制 DC 此更改或任何干预是否需要的如服务重启。  
  
-   作为复制的一部分，新 DC 获取新的计算机标识和条款本身作为副本 DC 拓扑中。 验证是否应用程序取决于计算机的身份，如其的名称、 帐户、 SID，等等。 无法它自动适应景象变化令克隆上计算机的身份？ 如果该应用程序缓存的数据，请确保它不会不依赖于可能缓存的计算机身份数据。  
  
## <a name="what-is-interesting-for-application-vendors"></a>什么是有趣的应用程序供应商？  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
运行你的应用或服务的域控制器无法复制直到该应用程序或服务是：  
  
-   通过使用获取 ADDCCloningExcludedApplicationList Windows PowerShell cmdlet 添加到 CustomDCCloneAllowList.xml 文件  
  
--或  
  
-   从域控制器中删除  
  
首次运行时的用户获取 ADDCCloningExcludedApplicationList cmdlet，它将返回域控制器上运行，但不是默认支持复制的应用程序和服务的列表中的应用程序和服务的列表。 默认情况下，你的应用程序或服务将不会列出。 添加你的应用列表中的应用程序和服务可以安全地或服务复制，获取 ADDCCloningExcludedApplicationList cmdlet 再次使用-GenerateXML 选项，以便将其添加到 CustomDCCloneAllowList.xml 文件的用户运行。 有关详细信息，请参阅[第 2 步： 运行获取 ADDCCloningExcludedApplicationList cmdlet](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet)。  
  
### <a name="distributed-system-interactions"></a>分布式的系统交互  
通常服务的独立于本地计算机通过或时参与复制失败。 分布式的服务必须担心小段时间同时网络有两个主计算机。 这可能清单作为服务实例试图引入了克隆已注册为身份新供应商来自合作伙伴系统的信息。 或在同一时间使用不同的结果，这两个服务的实例可能将信息推入广告 DS 数据库。 例如，它是不确定的网络上的域控制器具有 Windows 测试技术 (WTT) 服务，两台计算机时，将与通讯哪台计算机。  
  
分布式 DNS 服务器服务中，对于克隆过程仔细可避免覆盖 DNS 记录的源域控制器时克隆域控制器开头新的 IP 地址。  
  
不应依赖计算机直到复制结尾删除所有的旧的身份。 新的域控制器提升内的新的上下文后，选择的 Sysprep 提供商运行清理其他计算机的状态。 例如，它是计算机的在此时删除旧的证书，也会更改可以访问计算机的加密机密。  
  
最大的因素而异复制的计时是多少的对象有并从 PDC 复制。 较旧的媒体增加完成复制到所需的时间。  
  
由于你的应用程序或服务未知，向左运行。 克隆过程不会更改非 Windows 服务的状态。  
  
此外，新的计算机中安装有不同于原来计算机的 IP 地址。 这些行为，可能会导致到服务或应用程序，具体取决于该服务或应用程序在此环境中的行为方式的副作用。  
  
## <a name="additional-scenarios-suggested-for-testing"></a>其他建议测试的方案  
  
### <a name="cloning-failure"></a>复制失败  
服务提供商应该测试这种情况，因为计算机时复制失败启动到目录服务维修模式 (DSRM)，安全模式的形式。 在此时计算机尚未完成复制。 某些状态可能已更改，并且某些状态可能保持从原始域控制器。 这种情况下，若要了解什么影响它会对你的应用程序的测试。  
  
若要促使克隆失败，尝试复制域控制器，而无需授予其要复制的权限。 在此情况下，计算机将仅更改的 IP 地址，并且仍有大部分来自原始域控制器其状态。 有关授予要复制的域控制器权限的详细信息，请参阅[第 1 步： 授予权限，要复制的源虚拟化的域控制器](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source)。  
  
### <a name="pdc-emulator-cloning"></a>复制 PDC 仿真器  
服务和应用程序供应商应该测试这种情况下，因为重启系统时 PDC 模拟器复制。 此外，大部分复制下临时身份允许与进行交互 PDC 模拟器克隆过程中的新克隆执行。  
  
### <a name="writable-versus-read-only-domain-controllers"></a>可写与仅阅读域控制器  
服务和应用程序供应商应该测试复制使用类型都相同的域控制器 (即，可写或仅阅读域控制器上) 服务计划进行运行。  
  


