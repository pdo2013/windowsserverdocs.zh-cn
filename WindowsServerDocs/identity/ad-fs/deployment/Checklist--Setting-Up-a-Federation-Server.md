---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 清单-设置联合服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8fed51679ecabd883cfdcdf01523e123a1ca5243
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408519"
---
# <a name="checklist-setting-up-a-federation-server"></a>清单：设置联合服务器

此清单包含准备运行 Windows Server® 2012 Active Directory 联合身份验证服务 \(AD FS @ no__t 中的联合服务器角色的服务器所必需的部署任务。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![setting 联合服务器 @ no__t-1Checklist：设置联合服务器 @ no__t-0  
  
||任务|参考|  
|-|--------|-------------|  
|![设置联合服务器](media/icon_checkboxo.gif)|在开始部署 AD FS 联合服务器之前，请查看;1. @no__t 0 个优点和缺点，可选择 "Windows 内部数据库 \(WID @ no__t" 或 "SQL Server" 来存储 AD FS 配置数据库 2. @no__tAD FS 部署拓扑类型和关联的服务器位置和网络布局建议。|![setting 联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定 AD FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![setting 联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看 AD FS 容量规划指南，确定应该在生产环境中使用的适当数量的联合服务器。|![setting 联合服务器的联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[容量规划](https://technet.microsoft.com/library/gg749917.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看有关组织中的联合服务器位置的 AD FS 设计指南中的信息|![setting 联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合服务器的位置](https://technet.microsoft.com/library/dd807069.aspx)<br /><br />![setting](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[要放置联合服务器](https://technet.microsoft.com/library/dd807127.aspx)的联合服务器|  
|![设置联合服务器](media/icon_checkboxo.gif)|确定备用 @ no__t-0alone 联合服务器或联合服务器场是否更适合你的部署。|@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在创建联合服务器时](https://technet.microsoft.com/library/dd807101.aspx)0setting 联合服务器<br /><br />@no__t-](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在创建联合服务器场时](https://technet.microsoft.com/library/dd807062.aspx)0setting 联合服务器|  
|![设置联合服务器](media/icon_checkboxo.gif)|确定是否将在帐户伙伴组织或资源伙伴组织中创建此新的联合服务器。|![setting 联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[在帐户伙伴中查看联合服务器的角色](https://technet.microsoft.com/library/dd807117.aspx)<br /><br />![setting 联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[检查资源伙伴中联合服务器的角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看有关联合服务器如何使用服务通信证书和令牌 @ no__t-0signing 证书安全验证客户端和联合服务器代理请求的信息。 **警告：** 尽管使用具有非限定主机名（如 https： \/ @ no__t-1myserver）的证书的一般做法，但这些证书没有安全价值，并且可以使攻击者模拟 AD FS 联合身份验证服务到企业客户端. 因此，建议使用完全限定的域名 \(FQDN @ no__t-1，如 https： \/\/myserver.contoso.com，只使用颁发给联合身份验证服务的 FQDN 的 SSL 证书。|![setting 联合服务器的联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[证书要求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看有关如何更新企业网络域名系统的信息 \(DNS @ no__t-1，以便能够成功地将名称解析为联合服务器。|![setting 联合服务器的联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[名称解析要求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|将成为联合服务器的计算机加入帐户伙伴林或资源伙伴林中的域，将使用该计算机对该林的用户进行身份验证或从信任林进行身份验证。 **注意：** 如果要在帐户伙伴组织中设置联合服务器，则必须先将该计算机加入到林中的任何域中，联合服务器将使用联合服务器对该林的用户进行身份验证或从信任林进行身份验证。|@no__t 0setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将计算机加入域](Join-a-Computer-to-a-Domain.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|在公司网络 DNS 中创建一条新的资源记录，将联合服务器的 DNS 主机名指向联合服务器的 IP 地址。|![setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[向联合服务器的企业&#40;DNS&#41;添加主机 a 资源记录](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 如果要将联合服务器添加到联合服务器场，则可能必须先将现有令牌 @ no__t-2signing 证书的私钥导出 @no__t 在场 @ 3on 中的第一个联合服务器，以便当其他联合服务器必须导入相同的证书时，可以使用证书的文件格式。<br /><br />如果你颁发的服务器身份验证证书可供多台计算机重复使用，则不需要导出私钥 \(without 需要导出 @ no__t; 或者你将为每个计算机获得唯一的服务器身份验证证书服务器场中的联合服务器。 **注意：** AD FS 管理 snap @ no__t-0in 是指作为服务通信证书的联合服务器的服务器身份验证证书。|@no__t 0setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[导出服务器身份验证证书的私钥部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|从证书颁发机构 \(CA @ no__t @no__t 获取服务器身份验证证书后，必须将证书文件导入到每个联合服务器的默认网站。 **注意：** 必须先在默认网站上安装此证书，然后才能使用 AD FS 联合服务器配置向导。|@no__t 0setting 联合服务器将](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[服务器身份验证证书导入到默认](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)网站|  
|![设置联合服务器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 作为从 CA 获取服务器身份验证证书的替代方法，可以使用 Internet Information Services \(IIS @ no__t 为联合服务器创建示例证书。 **警告：** 在生产环境中使用 no__t-0signed 服务器身份验证证书部署联合服务器不是最佳安全方案。|![setting 联合服务器 @ no__t-1IIS：创建 no__t-0Signed 服务器证书 @ no__t，然后完成过程将[服务器身份验证证书导入到默认](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)网站|  
|![设置联合服务器](media/icon_checkboxo.gif)|如果要在帐户伙伴组织中配置联合服务器场环境，则必须在服务器场将驻留并配置每个 Active Directory 域服务 \(AD DS @ no__t 中创建和配置专用服务帐户在场中使用此帐户的联合服务器。 通过执行此过程，您将允许企业网络上的客户端使用 Windows 集成身份验证对服务器场中的任何联合服务器进行身份验证。|![setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手动配置联合服务器场的服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|在将成为联合服务器的计算机上安装联合身份验证服务角色服务。|@no__t 0setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安装联合身份验证服务角色服务](Install-the-Federation-Service-Role-Service.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|使用 AD FS 联合服务器配置向导，将计算机上的 AD FS 软件配置为充当联合服务器角色。<br /><br />如果要设置 "no__t-0alone 联合服务器"、"在新场中创建第一个联合服务器" 或 "将计算机加入现有的联合服务器场"，请按照此过程操作。 **注意：** 对于联合 Web 单一标志 @ no__t-0On \(SSO @ no__t 设计，帐户伙伴组织和资源伙伴组织中必须至少有一个联合服务器。|@no__t 0setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建独立联合服务器](Create-a-Stand-Alone-Federation-Server.md)<br /><br />@no__t 0setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在联合服务器场中创建第一个联合服务器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<br /><br />![setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将联合服务器添加到联合服务器场](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|\(Optional @ no__t-1 使用 AD FS 管理 snap @ no__t-2in 添加和配置部署设计所需的必要 AD FS 证书。 有关何时使用 snap @ no__t-0in 添加或更改证书的详细信息，请参阅[联合服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)。|@no__t 0setting 联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[添加令牌签名证书](Add-a-Token-Signing-Certificate.md)<br /><br />@no__t 0setting 联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[添加令牌解密证书](Add-a-Token-Decrypting-Certificate.md)<br /><br />![setting 联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[设置服务通信证书](Set-a-Service-Communications-Certificate.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|如果这是你的组织中的第一台联合服务器，则配置联合身份验证服务以便符合 AD FS 设计。|![setting 联合服务器 @ no__t-1Checklist：配置帐户伙伴组织 @ no__t-0<br /><br />![setting 联合服务器 @ no__t-1Checklist：配置资源伙伴组织 @ no__t-0|  
|![设置联合服务器](media/icon_checkboxo.gif)|在客户端计算机上，验证联合服务器是否正常运行。|![setting 联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[验证联合服务器是否正常运行](Verify-That-a-Federation-Server-Is-Operational.md)| 
