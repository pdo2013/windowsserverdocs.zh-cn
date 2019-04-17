---
title: 使用高可用性部署 Windows Admin Center
description: 部署 Windows Admin Center (Project Honolulu) 的高可用性
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 802a7bd537cab22893abb7e6657c4be90346ef88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "9025029"
---
# 使用高可用性部署 Windows Admin Center

>适用于：Windows Admin Center、Windows Admin Center 预览版

你可以在故障转移群集中为 Windows Admin Center 网关服务提供高可用性部署 Windows Admin Center。 提供的解决方案是主动被动解决方案，其中 Windows Admin Center 的一个实例处于活动状态。 如果其中一个群集中节点出现故障，Windows Admin Center 顺利故障转移到另一个节点，从而使你继续无缝地管理你的环境中的服务器。 

[了解有关其他 Windows Admin Center 部署选项。](../plan/installation-options.md)

## 先决条件

- 在 Windows Server 2016 或 2019年上的 2 个或多个节点故障转移群集。 [了解有关部署故障转移群集详细信息](../../../failover-clustering/failover-clustering-overview.md)。
- 群集共享卷 (CSV) Windows Admin center 来存储在群集中的所有节点可以都访问的永久数据。 足以在 CSV 10 GB。
- [Windows Admin Center HA 脚本 zip 文件](https://aka.ms/WACHAScript)中的高可用性部署脚本。 下载包含到本地计算机的脚本的.zip 文件，然后复制脚本根据具体取决于下面的指南。
- 建议，但可选： 签名的证书.pfx & 密码。 你无需将群集节点上已经安装了证书-脚本将为你完成的。 如果你不提供一个，安装脚本生成自签名的证书，这将在 60 天后过期。

## 故障转移群集上安装 Windows Admin Center

1. 复制```Install-WindowsAdminCenterHA.ps1```到群集中节点的脚本。 下载或将 Windows Admin Center.msi 复制到同一个节点。
2. 连接到通过 RDP 和运行的节点```Install-WindowsAdminCenterHA.ps1```从以下参数与该节点的脚本：
    - `-clusterStorage`： 群集共享卷，Windows Admin Center 的数据存储的本地路径。
    - `-clientAccessPoint`： 选择将用于访问 Windows Admin Center 的名称。 例如，如果使用参数运行该脚本`-clientAccessPoint contosoWindowsAdminCenter`，你将通过访问访问 Windows Admin Center 服务 `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`： 可选。 一个或多个群集通用服务的静态地址。 
    - `-msiPath`： Windows Admin Center.msi 文件的路径。
    - `-certPath`： 可选。 证书.pfx 文件的路径。
    - `-certPassword`： 可选。 中提供证书.pfx SecureString 密码 `-certPath`
    - `-generateSslCert`： 可选。 如果你不想要提供的签名的证书，包括此参数标志，以生成自签名的证书。 请注意自签名的证书将在 60 天后过期。
    - `-portNumber`： 可选。 如果你未指定端口，端口 443 (HTTPS) 上部署网关服务。 若要使用不同的端口，指定此参数中。 请注意，是否你使用自定义端口 （443 除了的任何内容），你将通过转至 https://\<clientAccessPoint\>:\<port\> 访问 Windows Admin Center。

> [!NOTE]
> ```Install-WindowsAdminCenterHA.ps1```脚本支持```-WhatIf ```和```-Verbose```参数

### 示例

#### 安装与签名证书：

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### 使用自签名证书安装：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## 更新现有的高可用性安装

使用相同```Install-WindowsAdminCenterHA.ps1```脚本来更新你的高可用性部署，而不会丢失连接数据。

### 更新到新版本的 Windows Admin Center

当发布新版本的 Windows Admin Center 时，只需运行```Install-WindowsAdminCenterHA.ps1```再次使用唯一的脚本```msiPath```参数：

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### 更新使用 Windows Admin Center 的证书

你可以更新 Windows Admin Center 的高可用性部署在任何时候通过提供新证书的.pfx 文件使用的证书和密码。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

你还可能会在同时使用新的.msi 文件更新的 Windows Admin Center 平台更新证书。

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## 卸载

若要从故障转移群集中卸载 Windows Admin Center 的高可用性部署，将传递```-Uninstall```参数```Install-WindowsAdminCenterHA.ps1```脚本。

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## 疑难解答

日志将保存在 CSV (例如，C:\ClusterStorage\Volume1\temp) 的临时文件夹。