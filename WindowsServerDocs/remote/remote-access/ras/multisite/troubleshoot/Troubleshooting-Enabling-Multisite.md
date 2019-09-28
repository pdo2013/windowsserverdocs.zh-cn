---
title: 启用多站点疑难解答
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fc42040d68b8a22dcfc46aa30db3a2a3c3bc060a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367059"
---
# <a name="troubleshooting-enabling-multisite"></a>启用多站点疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍与 `Enable-DAMultisite` 命令相关的问题疑难解答信息。 要确认你收到的错误是否与启用多站点有关，请检查 Windows 事件日志的事件 ID 10051。  
  
## <a name="user-connectivity-issues"></a>用户连接问题  
如果配置不正确，当你启用多站点时，用户可能出现连接问题。  
  
**原因**  
  
在多站点部署中，Windows 10 和 Windows 8 客户端计算机可以在不同的入口点之间漫游。  Windows 7 客户端计算机必须与多站点部署中的特定入口点相关联。 如果客户端计算机不在正确的安全组中，它们的组策略设置便可能出错。  
  
**解决方案**  
  
DirectAccess 至少需要为所有 Windows 10 和 Windows 8 客户端计算机提供一个安全组;建议为每个域的所有 Windows 10 和 Windows 8 计算机使用一个安全组。 DirectAccess 还需要为每个入口点提供适用于 Windows 7 客户端计算机的安全组。 每个客户端计算机只能属于一个安全组。 因此，你应该确保 Windows 10 和 Windows 8 客户端的安全组只包含运行 Windows 10 或 Windows 8 的计算机，并且每个 Windows 7 客户端计算机属于相关入口点的单个专用安全组，Windows 10 或 Windows 8 客户端均不属于 Windows 7 安全组。  
  
在 " **DirectAccess 客户端安装**向导" 的 "**选择组**" 页上配置 Windows 8 安全组。 在 "**启用多站点部署**向导" 的 "**客户端支持**" 页上或在 "**添加入口点**" 向导的 "**客户端支持**" 页上配置 Windows 7 安全组。  
  
## <a name="kerberos-proxy-authentication"></a>Kerberos 代理身份验证  
**接收到错误**。 在多站点部署中不支持 Kerberos 代理身份验证。 你必须为 IPsec 用户身份验证使用计算机证书。  
  
**原因**  
  
启用多站点前，必须启用计算机证书身份验证。  
  
**解决方案**  
  
要启用计算机证书身份验证，请执行以下操作：  
  
1.  在远程访问管理控制台的详细信息窗格中，在 **“步骤 2 远程访问服务器”** 中，单击 **“编辑”** 。  
  
2.  在 **“远程访问服务器设置”** 向导的 **“身份验证”** 窗格中，选中 **“用户计算机证书”** 复选框，然后选择在你的部署中颁发证书的根或中间证书颁发机构。  
  
若要使用 Windows PowerShell 启用计算机证书身份验证，请使用 @no__t cmdlet 并指定*IPsecRootCertificate*参数。  
  
## <a name="ip-https-certificates"></a>IP-HTTPS 证书  
**接收到错误**。 DirectAccess 服务器使用自签名的 ip-https 证书。 请将 IP-HTTPS 配置为使用已知 CA 的签名证书。  
  
**原因**  
  
IP-HTTPS 证书是自签名证书。 在多站点部署中你不能使用自签名证书。  
  
**解决方案**  
  
要选择 IP-HTTPS 证书，请执行以下操作：  
  
1.  在远程访问管理控制台的详细信息窗格中，在 **“步骤 2 远程访问服务器”** 中，单击 **“编辑”** 。  
  
2.  在 **“远程访问服务器设置”** 向导的 **“网络适配器”** 页面中，在 **“选择用于验证 IP-HTTPS 连接的证书”** 下面，确保清除 **“使用 DirectAccess 自动创建的自签名证书”** 复选框，然后单击 **“浏览”** ，选择受信任 CA 颁发的证书。  
  
## <a name="network-location-server"></a>网络位置服务器  
  
-   **问题1**  
  
    **接收到错误**。 DirectAccess 配置为对网络位置服务器使用自签名证书。 请将网络位置服务器配置为使用来自 CA 的签名证书。  
  
    **原因**  
  
    网络位置服务器部署在远程访问服务器上并且使用自签名证书。 在多站点部署中你不能使用自签名证书。  
  
    **解决方案**  
  
    要选择网络位置服务器证书，请执行以下操作：  
  
    1.  在远程访问管理控制台的详细信息窗格中，从 **“步骤 3 基础结构服务器”** 中单击 **“编辑”** 。  
  
    2.  在 **“基础结构服务器设置”** 向导中，在 **“网络位置服务器”** 页面上的 **“网络位置服务器部署在远程访问服务器上”** 下面，确保清除 **“使用自签名证书”** 复选框，然后单击 **“浏览”** 以选择企业 CA 颁发的证书。  
  
-   **问题2**  
  
    **接收到错误**。 若要部署网络负载平衡群集或多站点部署，请使用与远程访问服务器的内部名称不同的使用者名称获取网络位置服务器的证书。  
  
    **原因**  
  
    网络位置服务器网站使用的证书使用者名称与远程访问服务器的内部名称相同。 这将引发名称解析问题。  
  
    **解决方案**  
  
    获取使用者名称不同于远程访问服务器内部名称的证书。  
  
    要配置网络位置服务器，请执行以下操作：  
  
    1.  在远程访问管理控制台的详细信息窗格中，从 **“步骤 3 基础结构服务器”** 中单击 **“编辑”** 。  
  
    2.  在 **“基础结构服务器设置”** 向导的 **“网络位置服务器”** 页面中，在 **“网络位置服务器部署在远程访问服务器上”** 下面，单击 **“浏览”** ，选择之前获取的证书。 该证书的使用者名称必须不同于远程访问服务器的内部名称。  
  
## <a name="windows-7-client-computers"></a>Windows 7 客户端计算机  
**收到警告**。 启用多站点时，为 DirectAccess 客户端配置的安全组不能包含 Windows 7 计算机。 要在多站点部署中支持 Windows 7 客户端计算机，请为各个入口点选择包含该客户端的安全组。  
  
**原因**  
  
在现有的 DirectAccess 部署中，已启用 Windows 7 客户端支持。  
  
**解决方案**  
  
DirectAccess 至少需要为所有 Windows 8 客户端计算机提供一个安全组，并为每个入口点提供适用于 Windows 7 客户端计算机的安全组。 每个客户端计算机只能属于一个安全组。 因此，你应该确保 Windows 8 客户端的安全组仅包含运行 Windows 8 的计算机，并且每个 Windows 7 客户端计算机属于相关入口点的单个专用安全组，并且没有 Windows 8 客户端属于 Windows 7 安全组。  
  
## <a name="active-directory-site"></a>Active Directory 站点  
**接收到错误**。 服务器 < server_name > 未与 Active Directory 站点关联。  
  
**原因**  
  
DirectAccess 无法确定 Active Directory 站点。 在 Active Directory 站点和服务控制台中，你可以为网络配置不同子网并将各个子网与相关的 Active Directory 站点关联。 如果远程访问服务器的 IP 地址不属于任何一个子网，或 IP 地址所属的子网未定义 Active Directory 站点，则可能出现此错误。  
  
**解决方案**  
  
确认此问题是否是由于在远程访问服务器上运行 `nltest /dsgetsite` 命令而产生的。 如果是，该命令将返回 ERROR_NO_SITENAME。 要解决此问题，请确认你的域控制器上存在包含内部服务器 IP 地址的子网，并且该子网定义了 Active Directory 站点。  
  
## <a name="SaveGPOSettings"></a>正在保存服务器 GPO 设置  
**接收到错误**。 将远程访问设置保存到 GPO < GPO_name > 时出错。  
  
**原因**  
  
无法保存对服务器 GPO 的更改，原因可能是连接问题或 registry.pol 文件上存在共享冲突，例如，另一个用户已锁定该文件。  
  
**解决方案**  
  
请确保远程服务器和域控制器之间的连接正确。 如果连接没有问题，请检查域控制器的 registry.pol 文件是否被其他用户锁定，必要时，结束该用户会话，以解锁文件。  
  
## <a name="InternalServerError"></a>出现内部错误  
**接收到错误**。 出现内部错误。  
  
**原因**  
  
这可能是由于客户端　GPO　中的入口点表意外配置导致的。 如果管理员使用 DirectAccess 客户端 cmdlet　编辑客户端　GPO 中的入口点表，则可能出现此问题。  
  
**解决方案**  
  
检查所有客户端 GPO 中的入口点表配置，修复多站点配置中不同客户端 GPO 实例与 DirectAccess 配置之间的所有不一致问题。 使用带有客户端 GPO 名称的 `Get-DaEntryPointTableItem` cmdlet 来获取客户端上的入口点表。 使用 `Get-NetIPHttpsConfiguration` cmdlet 获取所有入口点的所有 IP-HTTPS 配置文件。  
  
有关详细信息，请参阅 [Windows PowerShell 中的 DirectAccess 客户端 Cmdlet](https://technet.microsoft.com/library/hh848426)。  
  


