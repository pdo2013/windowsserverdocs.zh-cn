---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 清单-配置 AD FS 以使用 AD FS 1.x 中的声明
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1c952abc0bca5eadbfc14f3eda54d05826d50294
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408483"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>清单：将 AD FS 配置为使用 AD FS 1.x 中的声明

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>清单：将 AD FS 配置为使用 AD FS 1.x 中的声明  
此清单包括将 Windows Server 2012 中的 Active Directory 联合身份验证服务 @no__t FS @ no__t 联合身份验证服务配置为使用 AD FS 1 发送的声明所需的任务。*x*联合身份验证服务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
AD FS @ no__t-1Checklist 中的 @no__t 0consume 声明：将 AD FS 配置为使用 AD FS 1.x @ no__t 中的声明  
  
||任务|参考|  
|-|--------|-------------|  
|![使用从 AD FS 声明](media/icon_checkboxo.gif)|规划 Windows Server 2012 和早期版本的 AD FS 中 AD FS 之间的互操作性，并详细了解名称 ID 声明类型。|AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划与 AD FS 1.x 之间的互操作性](https://technet.microsoft.com/library/ff678040.aspx)@no__t 0consume 声明|  
|![使用从 AD FS 声明](media/icon_checkboxo.gif)|必须先在 AD FS 联合身份验证服务中创建声明提供方信任，然后才能与 AD FS 的早期版本进行互操作。 **注意：** 不能使用 AD FS 1 创建信任。*x*联合身份验证服务使用联合元数据。<br /><br />使用右侧链接中的过程设置信任时，必须在 "添加声明提供方信任向导" 中执行以下操作，以将此信任设置为与 AD FS 1 进行互操作。*x*联合身份验证服务：<br /><br />1.在 **选择数据源** 页上，选择 **输入数据有关信赖方信任手动**。<br />2.在 **选择配置文件** 页上，选择 **AD FS 1.0 和 1.1 配置文件**。<br />3.在 "**配置 URL** " 页上的 " **WS @ No__t-2Federation 被动 URL**" 下，键入 AD FS 1 中定义的**联合身份验证服务终结点 URL** 。*x*联合身份验证服务。<br />4.在 "**配置标识符**" 页上的 "**声明提供方信任标识符**" 下，键入 AD FS 1 中定义的**联合身份验证服务 URI** 。*x*联合身份验证服务。|![consume 声明的 AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[手动创建声明提供方信任](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![使用从 AD FS 声明](media/icon_checkboxo.gif)|在之前创建的声明提供方信任上，必须创建一个声明规则，该规则将接受从 AD FS 1.x 联合身份验证服务传入的声明，并传递、筛选或转换为名称 ID 声明类型。<br /><br />当名称 ID 声明类型传递，通过筛选，或转换，它可以用作输入到另一个规则或规则，以便它可以理解并由 Windows Server 2012 中 AD FS 联合身份验证服务。|@no__t-AD FS 的声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建一个规则以发送 AD FS 1.X 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![使用从 AD FS 声明](media/icon_checkboxo.gif)|请与 AD FS 1 的管理员联系。*x*联合身份验证服务并具有 AD FS 1 的管理员。*x*联合身份验证服务设置新的资源伙伴信任。 此外，该管理员提供联合身份验证服务 URI \(联合身份验证服务属性中\), ，联合身份验证服务终结点 URL，并导出的令牌\-签名证书文件 \(仅公共密钥与\)。 管理员将需要这些项目以设置信任。|N\/A|  
  

