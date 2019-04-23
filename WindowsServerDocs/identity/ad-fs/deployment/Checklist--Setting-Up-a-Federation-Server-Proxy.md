---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: 清单-设置联合服务器代理
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57951787b1ac7694cacca170b376087a60e34543
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844048"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>清单：设置联合服务器代理

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

此清单包括准备 Active Directory 联合身份验证服务中的联合身份验证服务器代理角色运行 Windows Server® 2012年的服务器的部署任务\(AD FS\)。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![设置联合的代理服务器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：设置联合服务器代理**  
  
||任务|参考|  
|-|--------|-------------|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|在开始部署 AD FS 联合服务器代理之前，请查看 AD FS 部署拓扑类型及其关联的服务器的位置和网络布局建议。|![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定 AD FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合服务器代理位置](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[放置联合服务器代理的位置](https://technet.microsoft.com/library/dd807048.aspx)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|查看 AD FS 容量规划指南以确定应在生产环境中使用的联合服务器代理的正确号码。|![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证服务器代理容量规划](https://technet.microsoft.com/library/gg749898.aspx)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|确定一个联合服务器代理或联合服务器代理场是否更适合你的部署。 **注意：** 联合身份验证服务器还执行联合身份验证服务器代理职责。|![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联合服务器代理](https://technet.microsoft.com/library/dd807032.aspx)<br /><br />![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联合服务器代理场](https://technet.microsoft.com/library/dd807082.aspx)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|确定是否将在帐户伙伴组织或资源伙伴组织的外围网络中创建此新的联合服务器代理。|![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看的帐户伙伴中联合服务器代理角色](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看的资源伙伴中联合服务器代理角色](https://technet.microsoft.com/library/dd807052.aspx)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|将成为联合服务器代理的计算机上安装 AD FS 之前，请阅读有关获取服务器身份验证证书的重要性，针对联合身份验证服务器代理场-添加或场中的所有服务器之间共享证书。|![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合服务器代理的证书要求](https://technet.microsoft.com/library/dd807054.aspx)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|查看 AD FS 设计指南中有关如何更新域名系统的信息\(DNS\)以便可以进行成功的名称解析为联合身份验证服务器和联合服务器代理在外围网络中。|![设置联合的代理服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|确定联合服务器代理是否必须加入到域。 尽管联合服务器代理不需要加入到域，但它们都更轻松地加入到域时，使用远程管理和组策略功能管理。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将计算机加入到域](Join-a-Computer-to-a-Domain.md)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|具体取决于在外围网络中的 DNS 基础结构的配置方式，完成右侧的主题中的过程之一才能在组织中部署联合服务器代理。 **注意：** 不执行这两个过程。 读取[联合服务器代理的名称解析要求](https://technet.microsoft.com/library/dd807055.aspx)来确定最佳的过程适合你组织的需求。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[DNS 区域提供服务仅为外围网络中联合服务器代理的配置名称解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[配置联合服务器代理在 DNS 区域，提供两个外围网络和 Internet 客户端中的名称解析](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|获取服务器身份验证证书后，你必须安装它在 Internet Information Services \(IIS\)联合身份验证服务器代理的默认 Web 站点上。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[服务器身份验证证书导入到默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|\(可选\)作为从证书颁发机构获取服务器身份验证证书的替代方法\(CA\)，可以使用 IIS 为联合服务器代理获取示例证书。<br /><br />由于 IIS 生成自\-不来自受信任的源，则使用它来创建自签名的证书\-签名证书仅在以下方案中：<br /><br />-当您必须创建安全套接字层\(SSL\)你的服务器和用户的有限，已知组之间的通道<br />-当您需要第三个排查\-参与方证书问题**警告：** 不安全的最佳部署联合服务器代理在生产环境中使用自\-已签名，服务器身份验证证书。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS:创建自\-签名服务器证书](https://go.microsoft.com/fwlink/?LinkID=108271)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|在将成为联合服务器代理的计算机上安装联合身份验证服务代理角色服务。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安装联合身份验证服务代理角色服务](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|若要通过使用 AD FSFederation 服务器代理配置向导充当联合身份验证服务器代理角色的计算机上配置 AD FS 软件。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将计算机配置为联合服务器代理角色](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![设置联合的代理服务器](media/icon_checkboxo.gif)|使用事件查看器，验证已启动联合服务器代理服务。|![设置联合的代理服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[验证联合服务器代理是否正常运行](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  
