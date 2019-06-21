---
title: 使用高可用性部署 Windows Admin Center
description: 使用高可用性 (项目 Honolulu) 部署 Windows Admin Center
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ad8e2a8eade1ea9d3faaba8f387b1f489854e589
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280634"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>将 Windows Admin Center 部署具有高可用性

>适用于：Windows Admin Center，Windows Admin Center 预览版

你可以在故障转移群集来为 Windows Admin Center 网关服务提供高可用性部署 Windows Admin Center。 提供的解决方案是一种主动-被动解决方案，其中只有一个实例的 Windows Admin Center 处于活动状态。 如果一个群集中节点出现故障，Windows Admin Center 正常故障转移到另一个节点，让你继续无缝地管理您的环境中的服务器。 

[详细了解其他 Windows Admin Center 部署选项。](../plan/installation-options.md)

## <a name="prerequisites"></a>系统必备

- 2 个或多个节点上 Windows Server 2016 或 2019年的故障转移群集。 [详细了解如何部署故障转移群集](../../../failover-clustering/failover-clustering-overview.md)。
- 群集共享卷 (CSV) 的 Windows Admin Center 来存储可由群集中的所有节点访问的持久性数据。 10 GB 将足以满足你的 CSV。
- 从高可用性部署脚本[Windows Admin Center HA 脚本的 zip 文件](https://aka.ms/WACHAScript)。 下载包含脚本保存到本地计算机的.zip 文件，然后复制该脚本根据需要基于下面的指南。
- 可选但建议，： 签名的证书.pfx 和密码。 无需在群集节点上已安装证书-该脚本将为您完成的。 如果未提供，安装脚本生成自签名的证书，它将在 60 天后过期。

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>故障转移群集上安装 Windows Admin Center

1. 复制```Install-WindowsAdminCenterHA.ps1```将脚本保存到你的群集中的节点。 下载或将 Windows Admin Center.msi 复制到同一个节点。
2. 连接到通过 RDP 和运行节点```Install-WindowsAdminCenterHA.ps1```脚本从该节点具有以下参数：
    - `-clusterStorage`： 群集共享卷以存储 Windows Admin Center 的数据的本地路径。
    - `-clientAccessPoint`： 选择将用于访问 Windows Admin Center 的名称。 例如，如果使用参数运行该脚本`-clientAccessPoint contosoWindowsAdminCenter`，您将访问 Windows Admin Center 服务，请访问 `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`：可选。 有关群集通用服务的一个或多个静态地址。 
    - `-msiPath`：Windows Admin Center.msi 文件的路径。
    - `-certPath`：可选。 证书.pfx 文件的路径。
    - `-certPassword`：可选。 中提供的证书.pfx SecureString 密码 `-certPath`
    - `-generateSslCert`：可选。 如果不想要提供的签名的证书，包括此参数标志可生成自签名的证书。 请注意，自签名的证书将在 60 天后过期。
    - `-portNumber`：可选。 如果不指定端口，端口 443 (HTTPS) 上部署网关服务。 若要使用其他端口，指定此参数中。 请注意，如果使用自定义端口 （任何除 443)，您可以通过转到 https:// 访问 Windows Admin Center\<clientAccessPoint\>:\<端口\>。

> [!NOTE]
> ```Install-WindowsAdminCenterHA.ps1```脚本支持```-WhatIf ```和```-Verbose```参数

### <a name="examples"></a>示例

#### <a name="install-with-a-signed-certificate"></a>使用签名证书安装：

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>使用自签名证书安装：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>更新现有的高可用性安装

使用相同```Install-WindowsAdminCenterHA.ps1```脚本以更新 HA 部署，而不会丢失您连接的数据。

### <a name="update-to-a-new-version-of-windows-admin-center"></a>更新到新版本的 Windows Admin Center

Windows Admin Center 的新版本发布时，只需运行```Install-WindowsAdminCenterHA.ps1```再次使用仅具有脚本```msiPath```参数：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>更新 Windows Admin Center 使用的证书

可以更新通过提供新证书的.pfx 文件和密码的 Windows Admin Center HA 部署在任何时间使用的证书。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

您可能会使用新的.msi 文件中更新 Windows Admin Center 平台的同时更新的证书。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>卸载

若要从故障转移群集中卸载 Windows Admin Center HA 部署，请将传递```-Uninstall```参数```Install-WindowsAdminCenterHA.ps1```脚本。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>疑难解答

日志保存在 CSV (例如，C:\ClusterStorage\Volume1\temp) 的临时文件夹中。