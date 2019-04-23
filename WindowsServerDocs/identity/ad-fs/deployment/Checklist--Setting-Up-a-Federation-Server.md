---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: 清单-设置联合身份验证服务器
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 44448088184d86874e91855d8a51ef40d8cea049
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880498"
---
# <a name="checklist-setting-up-a-federation-server"></a>清单：设置联合服务器

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

此清单包括准备 Active Directory 联合身份验证服务中的联合身份验证服务器角色的运行 Windows Server® 2012年的服务器需要的部署任务\(AD FS\)。  
  
> [!NOTE]  
> 请按顺序完成本清单中的任务。 当某个参考连接将你转至某个过程时，应在完成该过程中的步骤之后返回此主题，以便你可以继续执行此清单中的其他任务。  
  
![设置联合服务器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**核对清单：设置联合身份验证服务器**  
  
||任务|参考|  
|-|--------|-------------|  
|![设置联合服务器](media/icon_checkboxo.gif)|在开始部署 AD FS 联合身份验证服务器之前，请查看;1.\)选择这两个 Windows 内部数据库的优点和缺点\(WID\)或 SQL Server 存储 AD FS 配置数据库 2。\)AD FS 部署拓扑类型和关联的服务器位置和网络布局建议。|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定 AD FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看 AD FS 容量规划指南以确定应在生产环境中使用的联合身份验证服务器的正确号码。|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证服务器容量规划](https://technet.microsoft.com/library/gg749917.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看 AD FS 设计指南中有关在何处放置你的组织中的联合身份验证服务器的信息|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合服务器的位置](https://technet.microsoft.com/library/dd807069.aspx)<br /><br />![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证服务器的放置位置](https://technet.microsoft.com/library/dd807127.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|确定是否独立\-单独使用联合身份验证服务器或联合服务器场是更好地为你的部署。|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联合身份验证服务器](https://technet.microsoft.com/library/dd807101.aspx)<br /><br />![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联合服务器场](https://technet.microsoft.com/library/dd807062.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|确定是否将在帐户伙伴组织或资源伙伴组织中创建此新的联合身份验证服务器。|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看的帐户伙伴中的联合服务器角色](https://technet.microsoft.com/library/dd807117.aspx)<br /><br />![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看的资源伙伴中的联合服务器角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看有关联合身份验证服务器服务通信证书和令牌的使用信息\-签名证书来安全地进行身份验证客户端和联合身份验证服务器代理请求。 **警告：** 尽管它很长时间以来常见的做法将证书与非限定的主机名称，例如 https:\/\/myserver，这些证书没有安全价值，并可以让攻击者模拟到 AD FS 联合身份验证服务企业客户端。 因此，建议使用完全限定的域名\(FQDN\)如 https:\/\/myserver.contoso.com 和颁发给联合身份验证服务 FQDN 的仅使用 SSL 证书。|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[的联合身份验证服务器证书要求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|查看有关如何更新企业网络域名系统信息\(DNS\)以便进行成功的名称解析到联合身份验证服务器。|![设置联合服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证服务器的名称解析要求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![设置联合服务器](media/icon_checkboxo.gif)|将成为帐户伙伴林中或在其中它将用于对该林的或从信任林中的用户进行身份验证的资源伙伴林中的域到联合身份验证服务器将计算机加入。 **注意：** 如果你想要设置帐户伙伴组织中的联合身份验证服务器，计算机必须先将加入到将使用你的联合身份验证服务器进行身份验证来自该林或从信任林中的用户的林中任何域。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将计算机加入到域](Join-a-Computer-to-a-Domain.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|在企业网络的联合身份验证服务器的 DNS 主机名指向联合身份验证服务器的 IP 地址的 DNS 中创建新的资源记录。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加主机&#40;A&#41;资源记录到公司 DNS 中为联合身份验证服务器](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|\(可选\)如果要向联合服务器场添加联合身份验证服务器，可能需要首先将导出的现有令牌的私钥\-签名证书\(场中的第一个联合身份验证服务器上\) ，以便您具有准备证书的文件格式时的其他联合服务器必须导入相同的证书。<br /><br />导出私钥时不需要多台计算机可以重复使用已颁发的服务器身份验证证书\(而无需导出\)或当您将获取的唯一的服务器身份验证证书场中的每个联合身份验证服务器。 **注意：** 在 AD FS 管理管理单元\-中是指用作服务通信证书的联合身份验证服务器的服务器身份验证证书。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[导出服务器身份验证证书的私钥部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|获取服务器身份验证证书后\(或私钥\)从证书颁发机构\(CA\)，然后必须将证书文件导每台联合服务器的默认 Web 站点。 **注意：** 在默认 Web 站点上安装此证书，一项要求，然后才能使用 AD FS 联合身份验证服务器配置向导。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[服务器身份验证证书导入到默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|\(可选\)作为从 CA 获得服务器身份验证证书的替代方法，您可以使用 Internet Information Services \(IIS\)创建示例联合身份验证服务器证书。 **警告：** 它不是安全的最佳做法，若要使用自部署在生产环境中的联合身份验证服务器\-签名服务器身份验证证书。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS:创建自\-签名服务器证书](https://go.microsoft.com/fwlink/?LinkID=108271)，然后完成该过程[服务器身份验证证书导入到默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|如果帐户伙伴组织中配置联合服务器场环境，必须创建并在 Active Directory 域服务中配置的专用的服务帐户\(AD DS\)场将驻留的位置和每台联合服务器场中配置为使用此帐户。 通过执行此过程，将在公司网络，以便对任何使用 Windows 集成身份验证场中的联合身份验证服务器进行身份验证允许客户端。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手动配置联合服务器场服务帐户](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|在将成为联合身份验证服务器的计算机上安装联合身份验证服务角色服务。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安装联合身份验证服务角色服务](Install-the-Federation-Service-Role-Service.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|若要通过使用 AD FS 联合身份验证服务器配置向导充当联合身份验证服务器角色的计算机上配置 AD FS 软件。<br /><br />按照以下过程，当你想要设置独立\-单独的联合身份验证服务器，在新场中创建第一个联合服务器或计算机加入到现有的联合服务器场。 **注意：** 对于联合 Web 单一登录\-上\(SSO\)设计中，您必须在帐户伙伴组织中的至少一台联合服务器和资源伙伴组织中的至少一台联合服务器。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建独立联合身份验证服务器](Create-a-Stand-Alone-Federation-Server.md)<br /><br />![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[在联合服务器场中创建第一个联合服务器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<br /><br />![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将联合身份验证服务器添加到联合服务器场](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|\(可选\)使用 AD FS 管理管理单元\-中添加和配置所需的 AD FS 证书所需部署你的设计。 有关何时添加或更改证书使用管理单元的详细信息\-中，请参阅[联合身份验证服务器的证书要求](https://technet.microsoft.com/library/dd807040.aspx)。|![设置联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[添加令牌签名证书](Add-a-Token-Signing-Certificate.md)<br /><br />![设置联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[添加令牌解密证书](Add-a-Token-Decrypting-Certificate.md)<br /><br />![设置联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[设置服务通信证书](Set-a-Service-Communications-Certificate.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|如果这是你组织中的第一个联合身份验证服务器，配置联合身份验证服务，以便它符合你的 AD FS 设计。|![设置联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：配置帐户伙伴组织](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![设置联合服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[核对清单：配置资源伙伴组织](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![设置联合服务器](media/icon_checkboxo.gif)|客户端计算机中，验证联合身份验证服务器正常运行。|![设置联合服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[验证联合身份验证服务器是否正常运行](Verify-That-a-Federation-Server-Is-Operational.md)| 
