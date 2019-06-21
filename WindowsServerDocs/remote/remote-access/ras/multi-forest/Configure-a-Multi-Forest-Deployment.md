---
title: Configure a Multi-Forest Deployment
description: 本主题是指南的 Windows Server 2016 中的多林环境中部署远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c8feff2-cae1-4376-9dfa-21ad3e4d5d99
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bf9222293dfd22b6f32cf00021f34b44c555e340
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281105"
---
# <a name="configure-a-multi-forest-deployment"></a>Configure a Multi-Forest Deployment

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍如何在几种可能的场景中配置远程访问多林部署。 所有场景均假定 DirectAccess 当前已部署在名为 Forest1 的单一林上，并且你正在配置 DirectAccess，以使其适用于名为 Forest2 的新林。  
  
## <a name="AccessForest2"></a>从 Forest2 访问资源  
在此场景中，DirectAccess 已部署在 Forest1 上，并且被配置为允许来自 Forest1 的客户端访问企业网络。 默认状态下，通过 DirectAccess 连接的客户端只能访问 Forest1 中的资源，而无法访问 Forest2 中的任何资源。  
  
#### <a name="to-enable-directaccess-clients-to-access-resources-from-forest2"></a>允许 DirectAccess 客户端访问 Forest2 的资源的步骤  
  
1.  如果 Forest2 的 DNS 后缀不是 Forest1 的 DNS 后缀的一部分，则添加带有 Forest2 的域名后缀的 NRPT 规则，或者向 DNS 后缀搜索列表中添加 Forest2 的域名后缀。  
  
2.  如果内部网络上部署了 IPv6，则添加 Forest2 中的相关内部 IPv6 前缀。  
  
## <a name="EnableForest2DA"></a>启用来自 Forest2 能够通过 DirectAccess 进行连接的客户端  
在此场景中，你对远程访问部署进行配置，以允许来自 Forest2 的客户端访问企业网络。 假定你已经为 Forest2 中的客户端计算机创建了所需的安全组。   
  
#### <a name="to-allow-clients-from-forest2-to-access-the-corporate-network"></a>允许来自 Forest2 的客户端访问企业网络的步骤  
  
1.  添加来自 Forest2 的客户端的安全组。  
  
2.  如果 Forest2 的 DNS 后缀不是 Forest1 的 DNS 后缀的一部分，添加 Forest2 以便启用对域控制器进行身份验证中的客户端的域后缀的 NRPT 规则和 （可选） 将 Forest2 中的域后缀添加到 DNS suf修复搜索列表。 
  
3.  添加 Forest2 中的内部 IPv6 前缀，以使 DirectAccess 能够创建通向域控制器的 IPsec 隧道，从而进行身份验证。  
  
4.  刷新管理服务器列表。  
  
## <a name="AddEPForest2"></a>从 Forest2 添加入口点  
在此场景中，DirectAccess 已部署在 Forest1 上的多站点配置中，并且你希望添加一个来自 Forest2 且名为 DA2 的远程访问服务器，作为现有 DirectAccess 多站点部署的入口点。  
  
#### <a name="to-add-a-remote-access-server-from-forest2-as-an-entry-point"></a>添加来自 Forest2 的远程访问服务器用作入口点的步骤  
  
1.  确保远程访问管理员拥有在 DA2 的域上写入 GPO 的足够权限，并且该远程访问管理员是 DA2 的本地管理员。  
  
2.  添加 DA2 作为入口点。   
  
3.  添加带有 Forest2 中的域名后缀的 NRPT 规则，以启用对域控制器的访问并进行身份验证，或者向 DNS 后缀搜索列表中添加 Forest2 的域名后缀。  
  
4.  添加 Forest2 中的相关内部 IPv6 前缀（若需要），以促使远程访问创建通向企业资源的 IPsec 隧道，并确保 NCSI 探测正确运行。  
  
5.  刷新管理服务器列表。  
  
## <a name="OTPMultiForest"></a>在多林部署中配置 OTP  
在多林部署中配置 OTP 时请注意以下术语：  
  
-   根 CA 林主 PKI 树 CA。  
  
-   企业 CA 的所有其他 Ca。  
  
-   资源林的林包含根 CA 并被视为管理林 \ 域。  
  
-   帐户林中的所有其他林拓扑中的。  
  
此程序需要 PowerShell 脚本 PKISync.ps1。 请参阅[AD CS:用于跨林证书注册的 PKISync.ps1 脚本](https://technet.microsoft.com/library/ff961506.aspx)。  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
### <a name="BKMK_CertPub"></a>将 Ca 配置为证书出版商  
  
1.  通过从提升的命令提示符运行以下命令，在所有林中的所有企业 CA 上启用 LDAP 引用支持：  
  
    ```  
    certutil -setreg Policy\EditFlags +EDITF_ENABLELDAPREFERRALS  
    ```  
  
2.  将所有企业 CA 计算机帐户添加到各个帐户林中的 Active Directory 证书发行者安全组。  
  
3.  通过从提升的命令提示符运行以下命令，重新启动所有林中所有 CA 计算机上的所有证书服务：  
  
    ```  
    net stop certsvc && net start certsvc  
    ```  
  
4.  通过从提升的命令提示符运行以下命令来提取根 CA 证书：  
  
    ```  
    certutil -config <Computer-Name>\<Root-CA-Name> -ca.cert <root-ca-cert-filename.cer>  
    ```  
  
    (如果根 CA 上运行该命令可以忽略连接信息-config < 计算机名 >\\< 根-CA-名称 >)  
  
    1.  通过从提升的命令提示符运行以下命令，在帐户林 CA 上从上一步导入根 CA 证书：  
  
        ```  
        certutil -dspublish -f <root-ca-cert-filename.cer> RootCA  
        ```  
  
    2.  为授予资源林证书模板读/写权限\<帐户林\>\\< 管理员帐户\>。  
  
    3.  通过从提升的命令提示符运行以下命令来提取所有资源林企业 CA 证书：  
  
        ```  
        certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
        ```  
  
        (如果根 CA 上运行该命令可以忽略连接信息-config < 计算机名 >\\< 根-CA-名称 >)  
  
    4.  通过从提升的命令提示符运行以下命令，在帐户林 CA 上从上一步导入企业 CA 证书：  
  
        ```  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> NTAuthCA  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> SubCA  
        ```  
  
    5.  从颁发的证书模板列表中删除帐户林 OTP 证书模板。  
  
### <a name="BKMK_DelImp"></a>删除和导入 OTP 证书模板  
  
1.  从帐户林删除 OTP 证书模板；即 Forest2。  
  
2.  使用以下 PowerShell 命令，将资源林的证书模板和 Oid 对象复制到各个帐户林：  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <DA OTP registration authority template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <Secure DA OTP logon certificate template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Oid -f  
    ```  
  
### <a name="BKMK_Publish"></a>发布 OTP 证书模板  
  
-   在所有帐户林 CA 上发布最新导入的证书模板。  
  
### <a name="BKMK_Extract"></a>提取和同步 CA  
  
1.  通过从提升的命令提示符运行以下命令，从帐户林提取所有企业 CA 证书：  
  
    ```  
    certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
    ```  
  
2.  使用以下 PowerShell 命令，将帐户林的各个林的 CA 与资源林的同步：  
  
    ```  
    .\PKISync.ps1 -sourceforest <account forest DNS> -targetforest <resource forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
3.  使用以下 PowerShell 命令，将资源林的各个林的 CA 与帐户林的同步：  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
## <a name="configuration-procedures"></a>配置过程  
以下各部分介绍上述场景部署的配置过程。 请在完成一个步骤后，返回场景以继续进行下一步。  
  
### <a name="NRPT_DNSSearchSuffix"></a>添加 NRPT 规则和 DNS 后缀  
通过 DirectAccess 与企业网络相连的客户端使用名称解析策略表 (NRPT) 来决定用来解析不同资源地址的 DNS 服务器。 这允许客户端解析企业资源地址，并帮助客户端维持一个正确的企业内外分类，这是保证 DirectAccess 运转所必须的。 DirectAccess 配置工具自动检测 Forest1 的根 DNS 后缀，并将其添加到 NRPT 表中。 但 Forest2 的 FQDN 后缀不会自动添加到 NRPT 表中，远程访问管理员必须手动添加。  
  
DNS 后缀搜索列表允许客户端不使用 FQDN，而使用短标签名。 远程访问配置工具自动将 Forest1 中的所有域添加到 DNS 后缀搜索列表中。 如果你想让客户端对 Forest2 中的资源使用短标签名，则需要手动添加。  
  
##### <a name="to-add-a-dns-suffix-to-the-nrpt-table-and-domain-suffixes-to-the-dns-suffix-search-list"></a>将 DNS 后缀添加到 NRPT 表以及将域名后缀添加到 DNS 后缀搜索列表的步骤  
  
1.  在远程访问管理控制台的中间窗格中，从 **“步骤 3 基础结构服务器”** 区域中，单击 **“编辑”** 。  
  
2.  在 **“网络位置服务器”** 页面上，单击 **“下一步”** 。  
  
3.  在 **“DNS”** 页的表格中，输入 Forest 2 中的企业网络所包含的任何其他名称后缀。 在 **“DNS 服务器地址”** 中，手动输入 DNS 服务器地址，或单击 **“检测”** 。 如果不输入地址，作为 NRPT 免除应用新条目。 然后，单击“下一步”  。  
  
4.  可选：在“DNS 后缀搜索列表”  页面上，通过在“新建后缀”  框中输入后缀来添加任何 DNS 后缀，然后单击“添加”  。 然后，单击“下一步”  。  
  
5.  在 **“管理”** 页面上，单击 **“完成”** 。  
  
6.  在远程访问管理控制台的中间窗格中，单击 **“完成”** 。  
  
7.  在“远程访问查看”  对话框中，单击“应用”  。  
  
8.  在 **“应用远程访问设置向导设置”** 对话框中，单击 **“关闭”** 。  
  
### <a name="IPv6Prefix"></a>添加内部 IPv6 前缀  
  
> [!NOTE]  
> 如果内部网络上部署了 IPv6，则只添加相关的内部 IPv6 前缀。  
  
远程访问管理着企业资源的 IPv6 前缀列表。 通过 DirectAccess 连接的客户端可能只可访问带有这些 IPv6 前缀的资源。 由于远程访问管理控制台和 Windows PowerShell 命令会自动添加 Forest1 的 IPv6 前缀，并且可能不添加其他林的前缀，必须手动添加任何缺失的 Forest2 前缀。  
  
##### <a name="to-add-an-ipv6-prefix"></a>添加 IPv6 前缀的步骤  
  
1.  在远程访问管理控制台的中间窗格中，从 **“步骤 2 远程访问服务器”** 区域，单击 **“编辑”** 。  
  
2.  在“远程访问服务器设置”向导中，单击 **“前缀配置”** 。  
  
3.  在 **“前缀配置”** 页面上，在 **“内部网络 IPv6 前缀”** 中，添加任何其他 IPv6 前缀，用分号分隔，例如 2001:db8:1::/64;2001:db8:2::/64。 然后，单击“下一步”  。  
  
4.  在 **“身份验证”** 页面上，单击 **“完成”** 。  
  
5.  在远程访问管理控制台的中间窗格中，单击 **“完成”** 。  
  
6.  在“远程访问查看”  对话框中，单击“应用”  。  
  
7.  在 **“应用远程访问设置向导设置”** 对话框中，单击 **“关闭”** 。  
  
### <a name="SGs"></a>添加客户端安全组  
若要启用来自 Forest2 通过 DirectAccess 访问资源的 Windows 8 客户端计算机，必须将 forest2 的安全组添加到远程访问部署。  
  
##### <a name="to-add-windows-8-client-security-groups"></a>添加 Windows 8 客户端安全组的步骤  
  
1.  在远程访问管理控制台的中间窗格中，从 **“步骤 1 远程客户端”** 区域，单击 **“编辑”** 。  
  
2.  在 DirectAccess 客户端设置向导中，单击 **“选择组”** ，然后在 **“选择组”** 页面上，单击 **“添加”** 。  
  
3.  在 **“选择组”** 对话框中，选择包含 DirectAccess 客户端计算机的安全组。 然后，单击“下一步”  。  
  
4.  在 **“网络连接助理”** 页面上，单击 **“完成”** 。  
  
5.  在远程访问管理控制台的中间窗格中，单击 **“完成”** 。  
  
6.  在“远程访问查看”  对话框中，单击“应用”  。  
  
7.  在 **“应用远程访问设置向导设置”** 对话框中，单击 **“关闭”** 。  
  
若要启用 Windows 7 启用多站点时通过 DirectAccess 访问资源来自 Forest2 的客户端计算机，必须将 forest2 的安全组添加到远程访问部署中每个入口点。 有关将 Windows 7 安全组添加的信息，请参阅的说明**客户端支持**3.6 中的页。 启用多站点部署。  
  
### <a name="RefreshMgmtServers"></a>刷新管理服务器列表  
远程访问自动发现包含 DirectAccess 配置 GPO 的所有林中的基础结构服务器。 如果 DirectAccess 是从 Forest1 的服务器部署的，则服务器 GPO 将被写入 Forest1 的域中。 如果你启用了 Forest2 的客户端对 DirectAccess 的访问，则客户端 GPO 将被写入 Forest2 的域中。  
  
要想实现通过 DirectAccess 访问域控制器和系统中心配置管理器，则需要基础结构服务器自动执行发现过程。 你必须手动启动发现过程。  
  
##### <a name="to-refresh-the-management-servers-list"></a>刷新管理服务器列表  
  
1.  在远程访问管理控制台中，单击 **“配置”** ，然后在 **“任务”** 窗格中，单击 **“刷新管理服务器”** 。  
  
2.  在 **“刷新管理服务器”** 对话框中，单击 **“关闭”** 。  
  


