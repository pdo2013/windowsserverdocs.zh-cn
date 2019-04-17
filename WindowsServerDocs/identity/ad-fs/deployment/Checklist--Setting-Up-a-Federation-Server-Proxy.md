---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: "清单-联合身份验证的服务器代理服务器设置"
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 997ee901172eb2d873ad02fa8ce39da09c5171c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>清单：联合身份验证的服务器代理服务器设置

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本文包含部署任务准备运行 Windows Server 的服务器® Active Directory 联合身份验证服务 \(AD FS\) 联盟服务器代理至关重要的 2012 年。  
  
> [!NOTE]  
> 完成此清单订单中的任务。 当参考链接将你带到过程时，返回到本主题之后你完成该过程中的步骤，以便你可以继续进行此清单中的其余任务。  
  
![联合的代理服务器设置](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**清单：联合身份验证的服务器代理服务器设置**  
  
||任务|参考|  
|-|--------|-------------|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|部署你广告 FS 联合身份验证的服务器代理之前，请查看广告 FS 部署拓扑类型及其关联的服务器位置和网络布局建议。|![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[确定您的广告 FS 部署拓扑](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联盟服务器代理布局](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[放置联合身份验证的服务器代理的位置](https://technet.microsoft.com/library/dd807048.aspx)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|查看广告 FS 容量规划指南若要确定应生产环境中使用的联合 server 代理正确数。|![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[规划联合身份验证的服务器代理容量](https://technet.microsoft.com/library/gg749898.aspx)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|确定是否更好地为你的部署单个联合身份验证的服务器代理或联合 server 代理场。 **注意：**联合身份验证的服务器还执行联合身份验证的服务器代理责任。|![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联合身份验证的服务器代理服务器](https://technet.microsoft.com/library/dd807032.aspx)<br /><br />![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[何时创建联盟服务器代理电场的日落](https://technet.microsoft.com/library/dd807082.aspx)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|确定是否将在外围帐户合作伙伴公司或资源合作伙伴公司的网络创建此新联合身份验证的服务器代理。|![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看联合服务器代理伙伴帐户中的角色](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[查看联合服务器代理资源伙伴中的角色](https://technet.microsoft.com/en-us/library/dd807052.aspx)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|您将成为联合身份验证的服务器代理计算机上安装广告 FS 之前，请阅读有关获取服务器身份验证证书的重要性，联合身份验证的服务器代理场-添加或跨电场的日落中的所有服务器共享证书。|![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证的服务器代理证书要求](https://technet.microsoft.com/library/dd807054.aspx)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|查看如何更新域名系统 \(DNS\) 外围网络中的，以便进行成功的名称分辨率的联合身份验证的服务器和联盟服务器代理广告 FS 设计指南中的信息。|![联合的代理服务器设置](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[联合身份验证的服务器代理名称分辨率要求](https://technet.microsoft.com/library/dd807055.aspx)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|确定是否必须联合身份验证的服务器代理加入域。 虽然联合身份验证的服务器代理无需连接到域，它们是更轻松地管理远程管理和组策略功能时加入域。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[计算机到某个域加入](Join-a-Computer-to-a-Domain.md)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|根据你外围网络中的 DNS 基础结构如何配置完成主题中的步骤之一上直接你之前在你的组织部署联合身份验证的服务器代理。 **注意：**无法执行这两个过程。 朗读[联合身份验证的服务器代理名称分辨率要求](https://technet.microsoft.com/library/dd807055.aspx)以确定哪些过程最佳适合你的组织的要求。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[配置的联合 Server 代理提供仅外围网络 DNS 区域中的名称分辨率](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[联盟服务器代理服务器 DNS 区域，有两个外围网络和 Internet 客户端中的配置名称分辨率](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|获取服务器身份验证证书后，你必须在联合身份验证的服务器代理服务器默认网站上的 Internet 信息服务 \(IIS\) 安装它。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[将服务器身份验证证书导入默认网站](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|作为向证书颁发机构获取服务器身份验证证书的替代 \(Optional\) \(CA\)，你可以使用 IIS 获取您联合身份验证的代理服务器服务器示例证书。<br /><br />由于 IIS 生成不来自受信任的源的 self\ 签名证书，可使用它来创建 self\ 签名证书，仅在以下情况下：<br /><br />-你必须时创建一个安全套接字层 \(SSL\) 频道服务器和受限、已知的一组用户之间<br />-时，你有 third\ 方证书的问题进行疑难解答**警告：**不安全的最佳做法，部署联合 server 代理生产的环境中使用 self\ 登录，服务器身份验证证书。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS：创建 Self\-Signed 服务器证书](https://go.microsoft.com/fwlink/?LinkID=108271)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|将成为联合身份验证的服务器代理计算机上安装联合身份验证服务代理角色服务。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[安装联合身份验证服务代理角色服务](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|配置为使用广告 FSFederation 服务器的代理配置向导充当联合身份验证的服务器代理角色计算机上的广告 FS 软件。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[配置为联合身份验证的服务器代理角色计算机](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![联合的代理服务器设置](media/icon_checkboxo.gif)|使用事件查看器，验证联合身份验证的服务器代理服务已经启动。|![联合的代理服务器设置](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[验证联盟服务器代理是操作](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  
