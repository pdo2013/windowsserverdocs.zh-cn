---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 清单-配置 AD FS 以使用 AD FS 1.x 中的声明
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f91944333da9ce4c1d78bbbf7b3652f1118e1f08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408491"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>清单：配置 AD FS 以将声明发送到 AD FS 1.x 联合身份验证服务

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>清单：配置 AD FS 以将声明发送到 AD FS 1.x 联合身份验证服务  
此清单包含在 Windows Server 2012 中配置 Active Directory 联合身份验证服务 \(AD FS @ no__t 联合身份验证服务以发送 AD FS 1 可以理解的声明所需的任务。*x*联合身份验证服务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![configure AD FS 发送声明 @ no__t-1Checklist：配置 AD FS 以将声明发送到 AD FS 1.x 联合身份验证服务 @ no__t-0  
  
||任务|参考|  
|-|--------|-------------|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|规划 Windows Server 2012 和早期版本的 AD FS 中 AD FS 之间的互操作性，并详细了解名称 ID 声明类型。|![configure AD FS 发送](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[与 AD FS 1.X 互操作性的声明规划](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|必须先在 AD FS 联合身份验证服务 AD FS 1 中创建信赖方信任，然后才能实现 AD FS 以前版本的互操作性。*x*联合身份验证服务。 **注意：** 不能使用 AD FS 1 创建信任。*x*联合身份验证服务使用联合元数据。<br /><br />使用右侧链接中的过程设置信任时，必须在 "添加信赖方信任向导" 中执行以下操作，以将此信任设置为与 AD FS 1 进行互操作。*x*联合身份验证服务：<br /><br />1.在 **选择数据源** 页上，选择 **输入数据有关信赖方信任手动**。<br />2.在 **选择配置文件** 页上，选择 **AD FS 1.0 和 1.1 配置文件**。<br />3.在 "**配置 URL** " 页上的 " **WS @ No__t-2Federation 被动 URL**" 下，键入 AD FS 1 中定义的**联合身份验证服务终结点 URL** 。*x*联合身份验证服务。<br />4.在 "**配置标识符**" 页上的 "**信赖部分信任标识符**" 下，键入 AD FS 1 中定义的**联合身份验证服务 URI** 。*x*联合身份验证服务。|![configure AD FS 发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[手动创建信赖方信任](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|在之前创建的信赖方信任上，你必须创建声明规则，这些规则将采用从属性存储提取的传入声明，并将其传递、筛选或转换为可由 AD 理解和使用的名称 ID 声明类型FS 1。*x*联合身份验证服务。 **注意：** 创建此规则之前，请确保你在其中创建此规则的声明规则集具有一个规则，该规则在第一次从属性存储中提取轻型目录访问协议 \(LDAP @ no__t 属性声明之前出现。 此声明将用作你创建的规则的输入，以发送 AD FS 1。*x*@no__t 1compatible 声明。 有关如何创建一个规则以提取 LDAP 属性的详细信息，请参阅 [创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![configure AD FS 发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建一个规则以发送 AD FS 1.X 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|请与 AD FS 1 的管理员联系。*x*联合身份验证服务并具有 AD FS 1 的管理员。*x*联合身份验证服务设置新的帐户伙伴信任。 此外，该管理员提供联合身份验证服务 URI \(联合身份验证服务属性中\), ，WS\-联合身份验证被动终结点 URL \(联合身份验证服务终结点 URL\), ，和一个导出的令牌\-签名证书文件 \(仅公共密钥与\)。 该管理员将需要这些项目以设置信任。|N\/A|  
  

