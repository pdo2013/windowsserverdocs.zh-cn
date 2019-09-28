---
title: 部署具有高可用性的 Windows 管理中心
description: 部署具有高可用性的 Windows 管理中心（Project Honolulu）
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406946"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>部署具有高可用性的 Windows 管理中心

>适用于：Windows Admin Center、Windows Admin Center 预览版

可以在故障转移群集中部署 Windows 管理中心，为 Windows 管理中心网关服务提供高可用性。 提供的解决方案是一个主动-被动解决方案，其中只有一个 Windows 管理中心实例处于活动状态。 如果群集中的一个节点发生故障，Windows 管理中心中心会正常故障转移到另一个节点，从而使你可以在环境中无缝地继续管理服务器。 

[了解其他 Windows 管理中心部署选项。](../plan/installation-options.md)

## <a name="prerequisites"></a>先决条件

- Windows Server 2016 或2019上2个或多个节点的故障转移群集。 [详细了解如何部署故障转移群集](../../../failover-clustering/failover-clustering-overview.md)。
- 用于 Windows 管理中心的群集共享卷（CSV）存储可由群集中的所有节点访问的持久数据。 对于 CSV，10 GB 就足够了。
- [Windows 管理中心 HA 脚本 zip 文件](https://aka.ms/WACHAScript)中的高可用性部署脚本。 将包含脚本的 .zip 文件下载到本地计算机，然后根据下面的指南按需复制该脚本。
- 建议，但可选：签名的证书 .pfx & 密码。 你不需要在群集节点上安装该证书-该脚本将为你执行此操作。 如果你未提供，则安装脚本会生成自签名证书，该证书将在60天后过期。

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>在故障转移群集上安装 Windows 管理中心

1. 将 ```Install-WindowsAdminCenterHA.ps1``` 脚本复制到群集中的节点。 下载 Windows 管理中心，或将其复制到相同的节点。
2. 通过 RDP 连接到节点，并从该节点运行具有以下参数的 ```Install-WindowsAdminCenterHA.ps1``` 脚本：
    - `-clusterStorage`：用于存储 Windows 管理中心数据的群集共享卷的本地路径。
    - `-clientAccessPoint`：选择将用于访问 Windows 管理中心的名称。 例如，如果使用参数 `-clientAccessPoint contosoWindowsAdminCenter` 运行该脚本，则将通过访问 `https://contosoWindowsAdminCenter.<domain>.com` 访问 Windows 管理中心服务
    - `-staticAddress`：可选。 群集通用服务的一个或多个静态地址。 
    - `-msiPath`：Windows 管理中心 .msi 文件的路径。
    - `-certPath`：可选。 证书的 .pfx 文件的路径。
    - `-certPassword`：可选。 @No__t 中提供的证书的 SecureString 密码。
    - `-generateSslCert`：可选。 如果你不希望提供签名证书，请包含此参数标志以生成自签名证书。 请注意，自签名证书将在60天后过期。
    - `-portNumber`：可选。 如果未指定端口，则会在端口443（HTTPS）上部署网关服务。 若要使用其他端口，请在此参数中指定。 请注意，如果使用自定义端口（443除外），则将通过转到 https://\<clientAccessPoint @ no__t： \<port @ no__t，来访问 Windows 管理中心。

> [!NOTE]
> @No__t-0 脚本支持 ```-WhatIf ``` 和 ```-Verbose``` 参数

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

使用同一个 ```Install-WindowsAdminCenterHA.ps1``` 脚本更新 HA 部署，而不会丢失连接数据。

### <a name="update-to-a-new-version-of-windows-admin-center"></a>更新到新版本的 Windows 管理中心

发布新版本的 Windows 管理中心后，只需使用 ```msiPath``` 参数即可再次运行 ```Install-WindowsAdminCenterHA.ps1``` 脚本：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>更新 Windows 管理中心使用的证书

可以通过提供新证书的 .pfx 文件和密码，随时更新 Windows 管理中心的 HA 部署所使用的证书。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

使用新的 .msi 文件更新 Windows 管理中心平台时，还可以更新证书。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>卸载

若要从故障转移群集中卸载 Windows 管理中心的 HA 部署，请将 ```-Uninstall``` 参数传递到 @no__t 脚本。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>疑难解答

日志保存在 CSV 的临时文件夹中（例如，C:\ClusterStorage\Volume1\temp）。