---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: 部署联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 689bd33bc95c2b142dfbe6d0448a604b2971979e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359979"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>清单：配置 AD FS 向 AD FS 1.x 声明感知 Web 代理发送声明

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>清单：配置 AD FS 以将声明发送到 AD FS 1.x 理赔 @ no__t-0aware Web 代理  
此清单包含在 Windows Server 2012 中配置 Active Directory 联合身份验证服务 \(AD FS @ no__t 联合身份验证服务以发送 Web 服务器托管的应用程序可以理解的声明所需的任务运行 AD FS 1。*x*声明 @ no__t-3aware Web 代理。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![configure AD FS 发送声明 @ no__t-1Checklist：配置 AD FS 以将声明发送到 AD FS 1.x 声明 @ no__t-0aware Web 代理 @ no__t-1  
  
||任务|参考|  
|-|--------|-------------|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|规划 Windows Server 2012 和早期版本的 AD FS 中 AD FS 之间的互操作性，并详细了解名称 ID 声明类型。|![configure AD FS 发送](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[与 AD FS 1.X 互操作性的声明规划](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|如果尚未执行此操作，请使用右侧的链接，在 Windows Server 2012 和 AD FS 1 的 AD FS 联合身份验证服务之间首先创建信赖方信任。*x*联合身份验证服务。|[清单：配置 AD FS 以将声明发送到 AD FS 1.x 联合身份验证服务](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|可以实现与 AD FS 1 托管的应用程序的互操作性。*x*声明 @ no__t-1aware Web 代理，必须首先在 Windows Server 2012 的 AD FS 联合身份验证服务中创建信赖方信任，然后才能 AD FS 1。 *x* 声明\-感知 Web 代理。 **注意：** 在 AD FS 联合身份验证服务中创建此信任等效于将新**应用程序**添加到 AD FS 1.x 联合身份验证服务 \(**联合身份验证服务 @ No__t-3Trust Policy @ No__t-4My 组织 @ no__t-5Application**\)。 此信赖方信任是必需的因为 AD FS 不具有等效值 **应用程序** 节点在其自己的管理单元中\-中。 但是，它仍然必须有一个到该应用程序安全通道。<br /><br />使用右侧链接中的过程设置信任时，必须在 "添加信赖方信任向导" 中执行以下操作，以将此信任设置为与 AD FS 1 进行互操作。*x*声明 @ no__t-1aware Web 代理：<br /><br />1.在 **选择数据源** 页上，选择 **输入数据有关信赖方信任手动**。<br />2.在 **选择配置文件** 页上，选择 **AD FS 1.0 和 1.1 配置文件**。<br />3.在 "**配置 URL** " 页上的 " **WS @ No__t-2Federation 被动 URL**" 下，键入 AD FS 1 中定义的**应用程序 URL** 。*x*联合身份验证服务。<br />4.在 "**配置标识符**" 页上的 "**信赖部分信任标识符**" 下，键入 AD FS 1 中定义的**应用程序 URL** 。*x*声明 @ no__t-4aware Web 代理|![configure AD FS 发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[手动创建信赖方信任](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|请与运行 AD FS 1 的 Web 服务器的管理员联系。*x*声明 @ no__t-1aware Web 代理，并让管理员编辑与声明 @ no__t 2aware 应用程序关联的 web.config 文件 \(under 默认网站 Internet Information Services \(IIS @ no__t-5 @ no__t将 Web 代理指向 AD FS 联合身份验证服务。<br /><br />例如，对于替换 *myresourcefederationserver* 标记中 `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` web.config 文件中使用有效的 AD FS 联合身份验证服务器名称。<br /><br />这对于应用程序和 AD FS 1.x 理赔 @ no__t-0aware Web agent 是必需的，以便能够使用从 Windows Server 2012 中的 AD FS 联合身份验证服务发送给它的声明。|N\/A|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|在之前创建的信赖方信任上，你必须创建声明规则，这些规则将采用从属性存储提取的传入声明，并将其传递、筛选或转换为名称 ID 声明类型，可供AD FS 1。*x*声明 @ no__t-1aware Web 代理。 **注意：** 创建此规则之前，请确保你在其中创建此规则的声明规则集具有一个规则，该规则在第一次从属性存储中提取轻型目录访问协议 \(LDAP @ no__t 属性声明之前出现。 此声明将用作你创建的规则的输入，以发送 AD FS 1。*x*@no__t 1compatible 声明。 有关如何创建一个规则以提取 LDAP 属性的详细信息，请参阅 [创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![configure AD FS 发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建一个规则以发送 AD FS 1.X 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

