---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: 使用 SQL Server 的联合服务器场
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 66c8bae2fbccca2bf618e46ffd3ccc05cb52f911
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191502"
---
# <a name="federation-server-farm-using-sql-server"></a>使用 SQL Server 的联合服务器场

此拓扑用于 Active Directory 联合身份验证服务\(AD FS\)方面不同于使用 Windows 内部数据库的联合服务器场\(WID\)部署拓扑中，它不会复制到数据场中的每个联合身份验证服务器。 相反，场中的所有联合身份验证服务器可以读取和写入到运行 Microsoft SQL Server 位于企业网络中的服务器存储的常见数据库的数据。  
  
## <a name="deployment-considerations"></a>部署注意事项  
本部分介绍有关目标的受众、 权益和限制，与此部署拓扑相关联的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>应使用此拓扑的用户？  
  
-   具有 100 多个需要向其内部用户和外部用户提供单一登录的信任关系的大型组织\-上\(SSO\)访问联合应用程序或服务  
  
-   组织已使用 SQL Server 并且想要充分利用其现有工具和专业知识  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   支持更大数量的信任关系\(超过 100 个\)  
  
-   对令牌重放检测的支持\(安全功能\)和项目解析\(安全断言标记语言的一部分\(SAML\) 2.0 协议\)  
  
-   对 SQL Server 的全部好处，如数据库镜像故障转移群集、 报告和管理工具的支持  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   默认情况下，此拓扑不提供数据库冗余。 尽管联合服务器场使用 WID 拓扑会自动将场中每个联合身份验证服务器上的 WID 数据库复制，SQL Server 拓扑的联合身份验证服务器场包含只有一个数据库副本  
  
> [!NOTE]  
> SQL Server 支持许多不同的数据和应用程序冗余选项，包括故障转移群集、 数据库镜像和多种不同类型的 SQL Server 复制。  
  
Microsoft 信息技术\(IT\)部门使用 SQL Server 数据库镜像在高\-安全\(同步\)模式和故障转移群集提供高\-SQL Server 实例的可用性支持。 SQL Server 事务\(对等方\-到\-对等方\)和合并复制不经过了 Microsoft 的 AD FS 产品团队。 有关 SQL Server 的详细信息，请参阅[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)或[选择适当类型的复制](https://go.microsoft.com/fwlink/?LinkId=214648)。  
  
### <a name="supported-sql-server-versions"></a>支持的 SQL Server 版本  
与 Windows Server 2012 一起安装的 AD FS 支持以下 SQL server 版本：  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>服务器位置和网络布局建议  
与联合服务器场以及 WID 拓扑类似，联合服务器场中的所有配置为使用一个群集域名系统\(DNS\)名称\(表示的联合身份验证服务名称\)和一个群集 IP 地址作为网络负载平衡的一部分\(NLB\)群集配置。 这有助于分配对单个联合身份验证服务器的客户端请求的 NLB 主机。 联合服务器代理可以用于将客户端请求代理到联合服务器场。  
  
下图显示了虚构的 Contoso Pharmaceuticals 公司部署其与企业网络中的 SQL Server 拓扑的联合服务器场的方式。 它还显示了该公司有权访问 DNS 服务器，使用相同的群集 DNS 名称的其他 NLB 主机配置外围网络的方式\(fs.contoso.com\)公司网络 NLB 群集，并使用两个使用联合服务器代理\(fsp1 和 fsp2\)。  
  
![使用 SQL server 场](media/FarmSQLProxies.gif)  
  
有关如何使用联合身份验证服务器或联合服务器代理配置为使用你的网络环境的详细信息，请参阅[联合身份验证服务器的名称解析要求](Name-Resolution-Requirements-for-Federation-Servers.md)或[名称联合服务器代理的解析要求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
