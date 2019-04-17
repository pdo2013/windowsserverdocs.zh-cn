---
title: 部署 Azure 中的 Windows Admin Center 网关
description: 如何部署 Azure 中的 Windows Admin Center 网关
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: af766c2e0502d9fe633cae42d999db5cbffc32c8
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296879"
---
# 部署 Azure 中的 Windows Admin Center

## 使用脚本部署

你可以下载[部署 WACAzVM.ps1](https://aka.ms/deploy-wacazvm)它将从[Azure 云 Shell](https://shell.azure.com)设置 Azure 中的 Windows Admin Center 网关运行。 此脚本可以创建整个环境，包括资源组。

[跳转到手动部署步骤](#deploy-manually-on-an-existing-azure-virtual-machine)

### 系统必备

* 设置你的帐户在[Azure 云 Shell](https://shell.azure.com)。 如果这是你第一次使用云 Shell，您将要求你进行关联，或使用云 Shell 创建 Azure 存储帐户。
* 在**PowerShell**云外壳程序中，导航到主页目录： ```PS Azure:\> cd ~```
* 若要上传```Deploy-WACAzVM.ps1```文件、 拖动和从本地计算机放到任意位置上云外壳窗口。

如果指定了自己的证书：

* 上传到[Azure 密钥保管库](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)的证书。 首先，在 Azure 门户中创建密钥保管库，然后上传到密钥保管库的证书。 或者，你可以使用 Azure 门户为你生成一个证书。

### 脚本参数

* **ResourceGroupName** -[字符串] 指定将在其中创建虚拟机资源组的名称。

* **名称**-[字符串] 指定的虚拟机的名称。

* **凭据**-[PSCredential] 指定的虚拟机的凭据。

* 部署现有虚拟机上的 Windows Admin Center 时， **MsiPath** -[字符串] 指定 Windows Admin Center MSI 的本地路径。 默认为从版本http://aka.ms/WACDownload如果省略。

* **VaultName** -[字符串] 指定包含证书的密钥保管库的名称。

* **CertName** -[字符串] 指定要用于 MSI 安装的证书的名称。

* **GenerateSslCert** -[开关] 如果 MSI 应生成自签名的 ssl 证书，则为 True。

* **端口号**-[int] 指定 Windows Admin Center 服务的 ssl 端口号。 默认为 443 如果省略。

* **OpenPorts** -[int []] 指定打开端口的虚拟机。

* **位置**-[字符串] 指定的虚拟机的位置。

* **大小**-[字符串] 指定的虚拟机大小。 默认值为"Standard_DS1_v2"如果省略。

* **图像**-[字符串] 指定的虚拟机的图像。 默认值为"Win2016Datacenter"如果省略。

* **VirtualNetworkName** -[字符串] 指定的虚拟机的虚拟网络的名称。

* **SubnetName** -[字符串] 指定的虚拟机的子网的名称。

* **SecurityGroupName** -[字符串] 的虚拟机指定安全组的名称。

* **PublicIpAddressName** -[字符串] 指定的虚拟机的公共 IP 地址的名称。

* **InstallWACOnly** -[开关] 设置为 True，如果 WAC 应在预先存在的 Azure VM 上安装。

有两个不同选项 MSI 部署和用于 MSI 安装的证书。 也可以从 aka.ms/WACDownload 下载 MSI，或者，如果部署到现有虚拟机，可以获得的 VM 在本地 MSI 文件路径。 可以在任何一种 Azure 密钥保管库中找到的证书或将由 MSI 生成自签名的证书。

### 脚本示例

首先，定义的参数的脚本所需的常见变量。

```PowerShell
$ResourceGroupName = "wac-rg1" 
$VirtualNetworkName = "wac-vnet"
$SecurityGroupName = "wac-nsg"
$SubnetName = "wac-subnet"
$VaultName = "wac-key-vault"
$CertName = "wac-cert"
$Location = "westus"
$PublicIpAddressName = "wac-public-ip"
$Size = "Standard_D4s_v3"
$Image = "Win2016Datacenter"
$Credential = Get-Credential
```

#### 示例 1： 使用脚本部署新的虚拟网络和资源组中新的 VM 上 WAC 网关。 使用从 aka.ms/WACDownload MSI 和从 MSI 的自签名的证书。

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### 示例 2： 与相同 #1，但使用来自 Azure 密钥保管库的证书。

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm2"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    VaultName = $VaultName
    CertName = $CertName
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### 示例 3： 使用现有虚拟机上本地 MSI 部署 WAC。

```PowerShell
$MsiPath = "C:\Users\<username>\Downloads\WindowsAdminCenter<version>.msi"
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm3"
    Credential = $Credential
    MsiPath = $MsiPath
    InstallWACOnly = $true
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

### 运行 Windows Admin Center 网关的虚拟机的要求

必须打开端口 443 (HTTPS)。
使用脚本定义的同一个变量，你可以使用下面的代码 Azure 云 Shell 中更新网络安全组：

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### 托管 Azure 虚拟机的要求

端口 5985 以实现 (通过 HTTP WinRM) 必须处于打开状态，并且具有活动侦听器。
你可以使用 Azure 云 Shell 中下面的代码更新托管的节点。 ```$ResourceGroupName``` 和```$Name```使用相同的变量作为部署脚本，但你将需要使用```$Credential```特定于虚拟机的管理。

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## 现有的 Azure 虚拟机上手动部署

之前在你所需的网关虚拟机上安装 Windows Admin Center，安装用于 HTTPS 通信的 SSL 证书或你可以选择使用由 Windows Admin Center 生成自签名的证书。 但是，你将尝试连接从浏览器，如果你选择第二种选项时收到一条警告。 选择**高级的 > 继续到 [网页]**，可以通过单击**详细信息 > 转到网页**，或在 Chrome，忽略此警告在 Edge 中。 我们建议你仅为测试环境中使用自签名的证书。

> [!NOTE]
> 不在服务器核心安装上安装具有桌面体验的 Windows Server 上的这些说明。 

1. [下载 Windows Admin Center](https://aka.ms/windowsadmincenter)到本地计算机。

2. 建立远程桌面连接到 VM，然后将 MSI 从本地计算机复制并粘贴到 VM。

3. 双击 MSI 开始安装，然后按照向导中的说明。 请注意以下事项：

   - 默认情况下，安装程序使用建议的端口 443 (HTTPS)。 如果你想要选择不同的端口，请注意，你需要打开该端口防火墙以及中。 

   - 如果你已经在 VM 上安装的 SSL 证书，请确保你选择该选项并输入指纹。

4. 启动 Windows Admin Center 服务 （运行 c: / 程序文件/Windows Admin Center/sme.exe）

[了解有关部署 Windows Admin Center 的更多信息。](../deploy/install.md)

### 配置网关 VM 启用 HTTPS 端口访问权限： 

1. 对虚拟机的 Azure 门户中导航并选择**网络**。 

2. 选择**添加入站的端口规则**并选择**HTTPS**下**服务**。 

> [!NOTE]
> 如果你选择了默认 443 以外的端口，选择**自定义**下服务和输入下**端口范围**的步骤 3 中所选的端口。 

### 访问 Azure VM 上安装 Windows Admin Center 网关

到目前为止，你应该能够访问 Windows Admin Center （边缘或 Chrome） 的现代浏览器从本地计算机上通过导航到你的网关 VM 的 DNS 名称。 

> [!NOTE]
> 如果你选择的端口 443 以外，你可以通过导航到你 VM\>:\<custom port\> https://\<DNS 名称访问 Windows Admin Center

在尝试访问 Windows Admin Center 时，浏览器将提示输入凭据，以访问在其安装 Windows Admin Center 在虚拟机。 下面你将需要输入本地用户或虚拟机的本地管理员组中的凭据。 

若要在 VNet 添加其他 Vm，请确保 WinRM 在 PowerShell 或命令提示符中运行以下命令在目标虚拟机上，目标虚拟机上运行： `winrm quickconfig`

如果你尚未加入域的 Azure 虚拟机，VM 行为类似于在工作组服务器中，因此你将需要确保你考虑[使用 Windows Admin Center 在工作组中](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。