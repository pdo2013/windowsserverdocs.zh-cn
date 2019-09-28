---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: 适用于应用程序供应商的虚拟化域控制器克隆测试指南
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2cafb040257b0fbc511e8225b0f07a2071012122
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360126"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>适用于应用程序供应商的虚拟化域控制器克隆测试指南

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题说明了应用程序供应商应该考虑哪些因素，以帮助确保在虚拟化域控制器（DC）克隆过程完成后，应用程序可以继续按预期方式工作。 它涵盖了需要进行额外测试的应用程序供应商和方案感兴趣的克隆过程的各个方面。 如果应用程序供应商验证其应用程序是否在已克隆的虚拟化域控制器上运行，则建议在本主题底部的社区内容中列出该应用程序的名称，以及指向组织的网站，用户可以在其中了解有关验证的详细信息。  
  
## <a name="overview-of-virtualized-dc-cloning"></a>虚拟化 DC 克隆概述  
[Active Directory 域服务（AD DS）虚拟化（级别100）](https://technet.microsoft.com/library/hh831734.aspx)和[虚拟化域控制器技术参考（级别300）](https://technet.microsoft.com/library/jj574214.aspx)简介中详细介绍了虚拟化域控制器克隆过程。 从应用程序供应商的角度来看，在评估克隆对应用程序的影响时，需要考虑以下事项：  
  
-   原始计算机未销毁。 它保留在网络上，与客户端进行交互。 与重命名不同，在删除原始计算机的 DNS 记录时，源域控制器的原始记录仍将保留。  
  
-   在克隆过程中，新计算机最初运行的是旧计算机标识下的一小段时间，直到启动克隆过程并做出必要的更改。 创建主机记录的应用程序应确保克隆的计算机不会在克隆过程中覆盖有关原始主机的记录。  
  
-   克隆是仅适用于虚拟化域控制器的特定部署功能，而不是用于克隆其他服务器角色的常规用途扩展。 有些服务器角色特别不支持克隆：  
  
    -   动态主机配置协议 (DHCP)  
  
    -   Active Directory 证书服务 (AD CS)  
  
    -   Active Directory 轻型目录服务 (AD LDS)  
  
-   在克隆过程中，会复制代表原始 DC 的整个 VM，因此也会复制该 VM 上的任何应用程序状态。 验证应用程序是否可以适应克隆的 DC 上本地主机的状态更改，或者是否需要任何干预（如服务重启）。  
  
-   在克隆过程中，新 DC 会获取新计算机标识，并在拓扑中将其自身设置为副本 DC。 验证应用程序是否依赖于计算机标识，例如其名称、帐户、SID 等。 它是否会自动适应克隆上计算机标识的更改？ 如果该应用程序缓存数据，请确保该应用程序不依赖于可能缓存的计算机标识数据。  
  
## <a name="what-is-interesting-for-application-vendors"></a>应用程序供应商有哪些兴趣？  
  
### <a name="customdccloneallowlistxml"></a>Customdccloneallowlist.xml  
运行应用程序或服务的域控制器将无法克隆，直到应用程序或服务可以：  
  
-   使用 Get-addccloningexcludedapplicationlist Windows PowerShell cmdlet 添加到 Customdccloneallowlist.xml 文件  
  
-或者-  
  
-   已从域控制器中删除  
  
用户首次运行 Get-addccloningexcludedapplicationlist cmdlet 时，将返回域控制器上运行的服务和应用程序的列表，但这些服务和应用程序不在支持克隆的服务和应用程序的默认列表中。 默认情况下，你的服务或应用程序将不会列出。 若要将你的服务或应用程序添加到可以安全克隆的应用程序和服务的列表，用户可以再次运行 Get-addccloningexcludedapplicationlist cmdlet 与-GenerateXML 选项，以便将其添加到 Customdccloneallowlist.xml 文件中。 有关详细信息，请参阅 [Step 2：运行 Get-addccloningexcludedapplicationlist cmdlet @ no__t-0。  
  
### <a name="distributed-system-interactions"></a>分布式系统交互  
通常，隔离于本地计算机的服务在参与克隆时通过或失败。 分布式服务必须考虑到一小段时间内网络上主计算机的两个实例。 这可能是一个服务实例，它尝试从已注册为该标识的新供应商的伙伴系统请求信息。 或服务的两个实例可以同时将信息推送到 AD DS 数据库中，结果不同。 例如，当具有 Windows 测试技术（WTT）服务的两台计算机位于域控制器的网络上时，不确定哪台计算机将与进行通信。  
  
对于分布式 DNS 服务器服务，当克隆域控制器以新的 IP 地址启动时，克隆过程会小心避免覆盖源域控制器的 DNS 记录。  
  
在克隆结束之前，不应依赖于计算机来删除所有旧标识。 新域控制器在新上下文中升级后，请选择 "Sysprep 提供程序运行" 以清理计算机的附加状态。 例如，此时将删除计算机的旧证书，并更改计算机可以访问的加密机密。  
  
改变克隆时间的最大因素是要从 PDC 复制的对象数量。 较旧的媒体将增加完成克隆所需的时间。  
  
由于你的服务或应用程序是未知的，因此它处于运行状态。 克隆过程不会更改非 Windows 服务的状态。  
  
此外，新计算机具有与原始计算机不同的 IP 地址。 这些行为可能会影响服务或应用程序，具体取决于服务或应用程序在此环境中的行为。  
  
## <a name="additional-scenarios-suggested-for-testing"></a>建议用于测试的其他方案  
  
### <a name="cloning-failure"></a>克隆失败  
服务供应商应该测试这种情况，因为当克隆失败时，计算机将启动到目录服务修复模式（DSRM），这种形式的安全模式。 此时，计算机尚未完成克隆。 某些状态可能已更改，某些状态可能会保留在原始域控制器中。 测试此方案，以了解它对应用程序的影响。  
  
若要导致克隆失败，请尝试克隆域控制器，而不授予其克隆权限。 在这种情况下，计算机将只更改 IP 地址，并且仍从原始域控制器获得其大部分状态。 有关授予域控制器权限进行克隆的详细信息，请参阅 [Step 1：授予源虚拟化域控制器克隆 @ no__t 的权限。  
  
### <a name="pdc-emulator-cloning"></a>PDC 仿真器克隆  
服务和应用程序供应商应该测试这种情况，因为在克隆 PDC 模拟器时，还需要重新启动。 此外，大多数克隆操作都是在临时标识下执行，以允许新克隆在克隆过程中与 PDC 仿真器交互。  
  
### <a name="writable-versus-read-only-domain-controllers"></a>可写域控制器和只读域控制器  
服务和应用程序供应商应该使用已计划在其上运行服务的相同类型的域控制器（即，在可写或只读的域控制器上）测试克隆。  
  


