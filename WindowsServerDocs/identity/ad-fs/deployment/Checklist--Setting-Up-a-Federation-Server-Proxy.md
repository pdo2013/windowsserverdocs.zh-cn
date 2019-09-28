---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: 清单-设置联合服务器代理
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 43fb8db4bf494ded93e592a1346a9ba99f0aff55
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359935"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>清单：设置联合服务器代理

此清单包括用于准备运行 Windows Server®2012的服务器的部署任务 Active Directory 联合身份验证服务 \(AD FS @ no__t 中的联合服务器代理角色。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![setting 联合代理服务器 @ no__t-1Checklist：设置联合服务器代理 @ no__t-0  
  
||任务|参考|  
|-|--------|-------------|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|开始部署 AD FS 联合服务器代理之前，请查看 AD FS 部署拓扑类型及其相关服务器布局和网络布局建议。|![setting 联合代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定 AD FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![setting 联合代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合服务器代理位置](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />@no__t-在](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[其中放置联合服务器代理](https://technet.microsoft.com/library/dd807048.aspx)的联合代理服务器0setting|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|查看 AD FS 容量规划指南，以确定应该在生产环境中使用的适当数量的联合服务器代理。|![setting 联合代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合服务器代理容量](https://technet.microsoft.com/library/gg749898.aspx)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|确定单个联合服务器代理或联合服务器代理场是否更适合你的部署。 **注意：** 联合服务器还执行联合服务器代理责任。|@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在创建联合服务器代理时](https://technet.microsoft.com/library/dd807032.aspx)0setting 联合代理服务器<br /><br />@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在创建联合服务器代理场时](https://technet.microsoft.com/library/dd807082.aspx)0setting 联合代理服务器|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|确定是否将在帐户伙伴组织或资源伙伴组织的外围网络中创建此新的联合服务器代理。|@no__t 0setting 联合代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在帐户伙伴中查看联合服务器代理的角色](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![setting 联合代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[检查资源伙伴中联合服务器代理的角色](https://technet.microsoft.com/library/dd807052.aspx)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|在将成为联合服务器代理的计算机上安装 AD FS 之前，请阅读获取服务器身份验证证书的重要性-对于联合服务器代理场-在服务器场中的所有服务器之间添加或共享证书。|![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合服务器代理](https://technet.microsoft.com/library/dd807054.aspx)的联合代理服务器证书要求|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|有关如何在外围网络中更新域名系统 \(DNS @ no__t 的详细信息，请查看 AD FS 设计指南中的信息，以便可以对联合服务器和联合服务器代理进行成功的名称解析。|![setting 联合服务器代理的联合代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|确定联合服务器代理是否必须加入域。 尽管不需要将联合服务器代理加入到域中，但在将它们加入域时，使用远程管理和组策略功能可以更轻松地管理它们。|![setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将计算机加入域](Join-a-Computer-to-a-Domain.md)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|根据外围网络中的 DNS 基础结构的配置方式，在组织中部署联合服务器代理之前，请先完成右侧主题中的一个过程。 **注意：** 请勿执行这两个过程。 读取[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)，以确定最适合组织需求的过程。|![setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在仅为外围网络提供服务的 DNS 区域中为联合服务器代理配置名称解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />@no__t 0setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在同时为外围网络和 Internet 客户端提供服务的 DNS 区域中为联合服务器代理配置名称解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|获取服务器身份验证证书后，必须将其安装在联合服务器代理的默认网站 Internet Information Services \(IIS @ no__t 中。|@no__t 0setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将服务器身份验证证书导入到默认](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)网站|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 作为从证书颁发机构获取服务器身份验证证书的替代方法 \(CA @ no__t，你可以使用 IIS 为联合服务器代理获取示例证书。<br /><br />由于 IIS 会生成一个不是来自受信任源的自 @ no__t-0signed 证书，因此仅在以下情况下使用它来创建一个自 @ no__t-1signed 证书：<br /><br />-必须在服务器与有限的一组已知用户组之间创建安全套接字层 \(SSL @ no__t<br />-当你必须排查第三个 @ no__t-0party 证书问题时，请**注意：** 在生产环境中使用自 @ no__t-0signed 服务器身份验证证书部署联合服务器代理不是最佳安全方案。|![setting 联合代理服务器 @ no__t-1IIS：创建 no__t-0Signed 服务器证书 @ no__t-1|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|在将成为联合服务器代理的计算机上安装联合身份验证服务代理角色服务。|@no__t 0setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安装联合身份验证服务代理角色服务](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|使用 AD FSFederation Server Proxy 配置向导将计算机上的 AD FS 软件配置为充当联合服务器代理角色。|![setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[为联合服务器代理角色配置计算机](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![设置联合代理服务器](media/icon_checkboxo.gif)|使用事件查看器，验证已启动联合服务器代理服务。|![setting 联合代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[验证联合服务器代理是否正常工作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  
