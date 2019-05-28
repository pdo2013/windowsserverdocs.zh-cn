---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 清单-配置 AD FS 以使用声明从 AD FS 1.x
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bd4c436c806074f63bf51f497429532d7be32f75
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192413"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>清单：配置 AD FS 以发送 AD FS 1.x 联合身份验证服务的声明

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>清单：配置 AD FS 以发送到 AD FS 声明 1.x 联合身份验证服务  
此清单包括配置 Active Directory 联合身份验证服务所需的任务\(AD FS\) Windows Server 2012，以发送声明可以理解的 AD FS 1 中的联合身份验证服务。*x*联合身份验证服务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![配置 AD FS 以发送声明](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：配置 AD FS 以发送到 AD FS 声明 1.x 联合身份验证服务**  
  
||任务|参考|  
|-|--------|-------------|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|Windows Server 2012 中的 AD FS 和以前版本的 AD FS 之间的互操作性的规划和了解更多有关名称 ID 声明类型。|![配置 AD FS 以发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划互操作性与 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|您可以实现与以前版本的 AD FS 互操作性之前，必须首先创建 AD FS 联合身份验证服务对 AD FS 1 中的信赖方信任。*x*联合身份验证服务。 **注意：** 不能使用 AD FS 1 创建信任。*x*联合身份验证服务使用联合身份验证元数据。<br /><br />如果您设置了信任关系使用右侧的链接中的过程，必须执行以下操作添加信赖方信任向导中设置此信任，以与 AD FS 1 进行互操作。*x*联合身份验证服务：<br /><br />1.在 **选择数据源** 页上，选择 **输入数据有关信赖方信任手动**。<br />2.在 **选择配置文件** 页上，选择 **AD FS 1.0 和 1.1 配置文件**。<br />3.上**配置 URL**页面上，在**WS\-联合身份验证被动 URL**，类型**联合身份验证服务终结点 URL** AD FS 1 中定义。*x*的合作伙伴联合身份验证服务。<br />4.上**配置标识符**页面上，在**信赖部分信任标识符**，类型**联合身份验证服务 URI** AD FS 1 中定义。*x*的合作伙伴联合身份验证服务。|![配置 AD FS 以发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[信赖方信任手动创建](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|在前面创建的信赖方信任，您必须创建声明规则，将需要从属性存储提取的传入声明和传递、 筛选器，或将其转换为名称 ID 声明类型可以理解和使用由 ADFS 1。*x*联合身份验证服务。 **注意：** 创建此规则之前，请确保在要创建此规则的声明规则集具有位于它之前它的第一次提取一种轻型目录访问协议的规则\(LDAP\)从属性存储的属性声明。 此声明将用作输入你创建的用于发送与 AD FS 1 的规则。*x*\-兼容的声明。 有关如何创建一个规则以提取 LDAP 属性的详细信息，请参阅 [创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![配置 AD FS 以发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建一个规则以发送 AD FS 1.x 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|与 AD FS 1 的管理员联系。*x*联合身份验证服务和 AD FS 1 与系统管理员。*x*设置新的帐户伙伴信任的联合身份验证服务。 此外，该管理员提供联合身份验证服务 URI \(联合身份验证服务属性中\), ，WS\-联合身份验证被动终结点 URL \(联合身份验证服务终结点 URL\), ，和一个导出的令牌\-签名证书文件 \(仅公共密钥与\)。 该管理员将需要这些项目以设置信任。|N\/A|  
  

