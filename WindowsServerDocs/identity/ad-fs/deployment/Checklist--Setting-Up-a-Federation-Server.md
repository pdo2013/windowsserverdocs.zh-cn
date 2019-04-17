---
ms.assetid: 8f954004-40d5-4c5e-8e0d-e8700c8ec7b1
title: "清单-设置联合身份验证的服务器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 44448088184d86874e91855d8a51ef40d8cea049
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-setting-up-a-federation-server"></a>清单：联合服务器设置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本文包含部署任务所需准备好运行 Windows Server 服务器® Active Directory 联合身份验证服务 \(AD FS\) 联盟服务器至关重要的 2012 年。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接将你带到过程时，返回到本主题之后你完成该过程中的步骤，以便你可以继续进行此清单中的其余任务。  
  
![设置联盟服务器](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：联合服务器设置**  
  
||任务|参考|  
|-|--------|-------------|  
|![联合服务器设置](media/icon_checkboxo.gif)|部署您广告 FS 联合身份验证的服务器之前，请查看;1。\) 的 Windows 内部数据库 \(WID\) 或 SQL Server 存储广告 FS 配置选择优缺点数据库 2。\) 广告 FS 部署拓扑类型和关联的服务器放置和网络布局建议。|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定您的广告 FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[广告 FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|查看广告 FS 容量规划指南若要确定应生产环境中使用的联合服务器正确数。|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合身份验证的服务器容量](https://technet.microsoft.com/library/gg749917.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|查看有关你的组织中放置联合身份验证的服务器广告 FS 设计指南中的信息|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合身份验证的服务器位置](https://technet.microsoft.com/library/dd807069.aspx)<br /><br />![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[放置联合服务器的位置](https://technet.microsoft.com/library/dd807127.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|确定是否更好地为你的部署 stand\ 单独联盟服务器或联合身份验证的服务器场。|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联盟服务器](https://technet.microsoft.com/library/dd807101.aspx)<br /><br />![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联盟服务器电场的日落](https://technet.microsoft.com/library/dd807062.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|确定是否将或资源合作伙伴公司帐户合作伙伴公司中创建此新联合身份验证的服务器。|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看联盟服务器伙伴帐户中的角色](https://technet.microsoft.com/library/dd807117.aspx)<br /><br />![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看联盟服务器资源伙伴中的角色](https://technet.microsoft.com/library/dd807065.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|查看有关联合身份验证的服务器方式使用服务通信证书和 token\ 签名证书牢固验证客户端和联合身份验证的服务器代理请求的信息。 **注意：**早已通常使用具有不合格的主机名称，如 https:\/\/myserver 证书，也是如此这些证书没有安全值，并且可以攻击者模拟广告 FS 联合身份验证服务用于企业客户端。 因此，我们建议你使用完整的域如 https:\/\/myserver.contoso.com 命名 \(FQDN\)，只使用 SSL 证书颁发给联合身份验证服务 FQDN。|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|了解有关如何更新以便进行成功的名称分辨率联合身份验证的服务器的企业网络域名系统 \(DNS\) 信息。|![设置联盟服务器](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证的服务器的名称分辨率要求](https://technet.microsoft.com/library/dd807041.aspx)|  
|![联合服务器设置](media/icon_checkboxo.gif)|加入家庭组将成为联盟服务器到某个域帐户合作伙伴森林或了它将使用该森林或从信任森林用户进行身份验证的资源合作伙伴树林中的计算机。 **注意：**如果你想要设置帐户合作伙伴公司中的联合服务器的计算机必须首次连接到任何将使用您联合身份验证的服务器来验证用户从该森林或信任森林森林中的域。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[计算机到某个域加入](Join-a-Computer-to-a-Domain.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|公司的网络 DNS 联合身份验证的服务器 DNS 主机名指向联合身份验证的服务器的 IP 地址创建新的资源记录。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加主机 & #40;一个 & #41;资源录制到公司 DNS 服务器联合身份验证](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|如果您将到联合身份验证的服务器场添加联盟服务器 \(Optional\)，你可能必须先导现有 token\ 签名证书的专用键 \（在第一个联盟服务器上 farm\ 中），以便你拥有的证书准备文件格式时其他联合身份验证的服务器必须导入同一个证书。<br /><br />导出专用键时不需要通过多台计算机可重复使用已颁发的服务器身份验证证书 \（无需 export\ 到）或会时获取独特的服务器身份验证证书的场中每个联合身份验证的服务器。 **注意：**广告 FS 管理 snap\ 中指联盟服务器服务通信证书为服务器身份验证证书。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[导出服务器身份验证证书专用键一部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|你将获得服务器身份验证证书后 \(or private key\) 向证书颁发机构 \(CA\)，你必须然后将文件导入证书默认网站为每个联合身份验证的服务器。 **注意：**之前，你可以使用该广告 FS 联盟服务器配置向导将默认网站上安装此证书是要求。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将服务器身份验证证书导入默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|除了从 CA 获得服务器身份验证证书 \(Optional\)，可以使用 Internet 信息服务 \(IIS\) 为您联合身份验证的服务器创建示例证书。 **注意：**不安全的最佳做法，使用 self\ 登录服务器身份验证证书的部署联盟服务器生产环境中的。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：创建 Self\-Signed 服务器证书](https://go.microsoft.com/fwlink/?LinkID=108271)，然后完成该过程[将服务器身份验证证书导入默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|如果准备帐户合作伙伴组织中配置联盟服务器电场的日落环境，你必须在创建和配置专用的服务帐户 Active Directory 域服务 \(AD DS\) 电场的日落将驻留何地配置场使用此帐户中的每个联合身份验证的服务器。 通过执行此过程，你将允许在进行任何中使用 Windows 的集成身份验证的场联盟服务器的身份验证的企业网络上的客户端。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[手动配置服务帐户联盟服务器电场的日落](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|将成为联盟服务器的计算机上安装联合身份验证服务角色服务。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安装联合身份验证服务角色服务](Install-the-Federation-Service-Role-Service.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|配置为使用广告 FS 联盟服务器配置向导充当联合身份验证的服务器角色计算机上的广告 FS 软件。<br /><br />当你想要单独 stand\ 联盟服务器设置、创建新电场的日落中的第一个联合身份验证的服务器或一台计算机加入现有联盟服务器场，请按照以下步骤。 **注意：**联合 Web 单个 Sign\-On \(SSO\) 设计，你必须帐户合作伙伴公司中的至少一个联合服务器和资源的合作伙伴公司中至少一个联合服务器。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建独立联盟服务器](Create-a-Stand-Alone-Federation-Server.md)<br /><br />![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[创建第一个联盟服务器联盟服务器电场的日落](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)<br /><br />![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[添加到联合身份验证的服务器场联合服务器](Add-a-Federation-Server-to-a-Federation-Server-Farm.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|\(Optional\) 部署设计，你所需使用该广告 FS 管理 snap\ 中添加和配置必要广告 FS 证书。 有关何时添加或更改 snap\ 在使用证书的详细信息，请参阅[服务器联合身份验证的证书要求](https://technet.microsoft.com/library/dd807040.aspx)。|![设置联盟服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[添加令牌签名证书](Add-a-Token-Signing-Certificate.md)<br /><br />![设置联盟服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[添加令牌解密证书](Add-a-Token-Decrypting-Certificate.md)<br /><br />![设置联盟服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[设置服务通信证书](Set-a-Service-Communications-Certificate.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|如果这是在你的组织的第一个联盟服务器，请以便使其符合你的广告 FS 设计配置联合身份验证服务。|![设置联盟服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：配置的帐户的合作伙伴公司](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![设置联盟服务器](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[清单：配置资源合作伙伴公司](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![联合服务器设置](media/icon_checkboxo.gif)|客户端计算机上，从验证联合身份验证的服务器在运行。|![设置联盟服务器](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[验证联盟服务器是操作](Verify-That-a-Federation-Server-Is-Operational.md)| 
