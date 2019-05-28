---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: 部署联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 874984b469303c0f8a40a676632c144ee6079f44
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192428"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>清单：配置 AD FS 以发送到 AD FS 1.x 声明感知 Web 代理声明

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>清单：配置 AD FS 以发送到 AD FS 1.x 声明声明\-感知 Web 代理  
此清单包括配置 Active Directory 联合身份验证服务所需的任务\(AD FS\) Windows Server 2012 中以发送声明可以理解的应用程序都由联合身份验证服务运行 AD FS 1 的 web 服务器。*x*声明\-感知 Web 代理。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![配置 AD FS 以发送声明](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：配置 AD FS 以发送到 AD FS 1.x 声明声明\-感知 Web 代理**  
  
||任务|参考|  
|-|--------|-------------|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|Windows Server 2012 中的 AD FS 和以前版本的 AD FS 之间的互操作性的规划和了解更多有关名称 ID 声明类型。|![配置 AD FS 以发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划互操作性与 AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|如果尚未这样做，使用以下链接在右侧首次创建 Windows Server 2012 中 AD FS 联合身份验证服务和 AD FS 1 之间的信赖方信任。*x*联合身份验证服务。|[清单：配置 AD FS 以将声明发送到 AD FS 1.x 联合身份验证服务](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|之前可以实现与 AD fs 1 托管的应用程序的互操作。*x*声明\-感知 Web 代理，你必须首先创建 AD FS 联合身份验证服务中的信赖方信任在 Windows Server 2012 中 AD fs 1。 *x* 声明\-感知 Web 代理。 **注意：** 创建此信任 AD FS 联合身份验证服务是添加一个新的等效**应用程序**到 AD FS 1.x 联合身份验证服务\(**联合身份验证服务\\信任策略\\我的组织\\应用程序**\)。 此信赖方信任是必需的因为 AD FS 不具有等效值 **应用程序** 节点在其自己的管理单元中\-中。 但是，它仍然必须有一个到该应用程序安全通道。<br /><br />如果您设置了信任关系使用右侧的链接中的过程，必须执行以下操作添加信赖方信任向导中设置此信任，以与 AD FS 1 进行互操作。*x*声明\-感知 Web 代理：<br /><br />1.在 **选择数据源** 页上，选择 **输入数据有关信赖方信任手动**。<br />2.在 **选择配置文件** 页上，选择 **AD FS 1.0 和 1.1 配置文件**。<br />3.上**配置 URL**页面上，在**WS\-联合身份验证被动 URL**，类型**应用程序 URL** AD FS 1 中定义。*x*的合作伙伴联合身份验证服务。<br />4.上**配置标识符**页面上，在**信赖部分信任标识符**，类型**应用程序 URL** AD FS 1 中定义。*x*声明\-感知 Web 代理|![配置 AD FS 以发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[信赖方信任手动创建](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|与运行 AD FS 1 的 Web 服务器的管理员联系。*x*声明\-感知 Web 代理，并具有编辑与声明关联的 web.config 文件的管理员\-识别的应用程序\(下将默认 Web 站点中的互联网信息服务\(IIS\) \)指向 AD FS 联合身份验证服务的 Web 代理。<br /><br />例如，对于替换 *myresourcefederationserver* 标记中 `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` web.config 文件中使用有效的 AD FS 联合身份验证服务器名称。<br /><br />这是必需的应用程序和 AD FS 1.x 声明\-感知 Web 代理，以便能够使用从 Windows Server 2012 中 AD FS 联合身份验证服务发送给它的声明。|N\/A|  
|![配置 AD FS 以发送声明](media/icon_checkboxo.gif)|在前面创建的信赖方信任，必须创建声明规则，将需要从属性存储提取的传入声明和传递、 筛选或转换它们转换为名称 ID 声明类型可以理解和使用情况AD FS 1。*x*声明\-感知 Web 代理。 **注意：** 创建此规则之前，请确保在要创建此规则的声明规则集具有位于它之前它的第一次提取一种轻型目录访问协议的规则\(LDAP\)从属性存储的属性声明。 此声明将用作输入你创建的用于发送与 AD FS 1 的规则。*x*\-兼容的声明。 有关如何创建一个规则以提取 LDAP 属性的详细信息，请参阅 [创建规则以声明方式发送 LDAP 属性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)。|![配置 AD FS 以发送声明](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[创建一个规则以发送 AD FS 1.x 兼容声明](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

