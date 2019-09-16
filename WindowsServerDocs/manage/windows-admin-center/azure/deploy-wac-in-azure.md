---
title: 在 Azure 中部署 Windows 管理中心网关
description: 如何在 Azure 中部署 Windows 管理中心网关
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d0ebc957715f88898a9c14d2841d8b820f862a0d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869137"
---
# <a name="deploy-windows-admin-center-in-azure"></a>在 Azure 中部署 Windows 管理中心

## <a name="deploy-using-script"></a>使用脚本进行部署

可以从 [Azure Cloud Shell](https://shell.azure.com) 中下载要从运行的[Deploy-WACAzVM ](https://aka.ms/deploy-wacazvm)，以便在 Azure 中设置 Windows 管理中心网关。 此脚本可以创建整个环境，包括资源组。

[跳转到手动部署步骤](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>先决条件

* 在[Azure Cloud Shell](https://shell.azure.com)中设置帐户。 如果这是你第一次使用 Cloud Shell，则会要求你将 Azure 存储帐户与 Cloud Shell 相关联或创建。
* 在**PowerShell** Cloud Shell 中，导航到主目录：```PS Azure:\> cd ~```
* 若要上```Deploy-WACAzVM.ps1```传文件，请将其从本地计算机拖放到 Cloud Shell 窗口中的任意位置。

如果指定自己的证书：

* 将证书上传到[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)。 首先，在 Azure 门户中创建密钥保管库，并将该证书上传到密钥保管库。 或者，可以使用 Azure 门户为你生成证书。

### <a name="script-parameters"></a>脚本参数

* **ResourceGroupName** -[String] 指定将在其中创建 VM 的资源组的名称。

* **Name** -[String] 指定 VM 的名称。

* **凭据**-[PSCredential] 指定 VM 的凭据。

* **MsiPath** -[String] 在现有 VM 上部署 windows 管理中心时，指定 WINDOWS 管理中心 MSI 的本地路径。 如果省略，则默认 http://aka.ms/WACDownload 为中的版本。

* **VaultName** -[String] 指定包含证书的密钥保管库的名称。

* **CertName** -[String] 指定要用于 MSI 安装的证书的名称。

* **GenerateSslCert** -[Switch] 如果 MSI 应生成自签名 ssl 证书，则为 True。

* **PortNumber** -[Int] 指定 Windows 管理中心服务的 ssl 端口号。 如果省略，则默认值为443。

* **OpenPorts** -[int []] 指定 VM 的开放端口。

* **Location** -[String] 指定 VM 的位置。

* **Size** -[字符串] 指定 VM 的大小。 如果省略，则默认为 "Standard_DS1_v2"。

* **Image** -[String] 指定 VM 的映像。 如果省略，则默认为 "Win2016Datacenter"。

* **VirtualNetworkName** -[String] 指定 VM 的虚拟网络的名称。

* **SubnetName** -[String] 指定 VM 的子网名称。

* **SecurityGroupName** -[String] 指定 VM 的安全组的名称。

* **PublicIpAddressName** -[String] 指定 VM 的公共 IP 地址的名称。

* **InstallWACOnly** -如果应在预先存在的 Azure VM 上安装 WAC，则将其设置为 True。

有2个不同的选项可供 MSI 部署和用于 MSI 安装的证书。 可以从 aka.ms/WACDownload 下载 MSI，或者，如果要部署到现有 VM，可以在 VM 上本地提供 MSI 的 filepath。 可以在 Azure Key Vault 中找到该证书，也可以使用 MSI 生成的自签名证书。

### <a name="script-examples"></a>脚本示例

首先，定义脚本参数所需的常见变量。

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>示例 1：使用脚本将 WAC 网关部署到新的虚拟网络和资源组中的新 VM。 使用 aka.ms/WACDownload 中的 MSI 和来自 MSI 的自签名证书。

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>示例 2：与 #1 相同，但使用 Azure Key Vault 中的证书。

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>示例 3：使用现有 VM 上的本地 MSI 部署 WAC。

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>运行 Windows 管理中心网关的 VM 的要求

必须打开端口443（HTTPS）。
使用为脚本定义的相同变量，可以使用下面的代码 Azure Cloud Shell 更新网络安全组：

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>托管 Azure VM 的要求

端口5985（通过 HTTP 的 WinRM）必须为打开状态，并且具有活动侦听器。
可以在 Azure Cloud Shell 中使用下面的代码来更新托管节点。 ```$ResourceGroupName```与```$Name```部署脚本使用相同的变量，但你需要```$Credential```使用特定于你所管理的 VM。

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>在现有 Azure 虚拟机上手动部署

在所需的网关 VM 上安装 Windows 管理中心之前，请安装用于 HTTPS 通信的 SSL 证书，也可以选择使用 Windows 管理中心生成的自签名证书。 但是，如果选择后一项选项，则当尝试从浏览器进行连接时，会收到警告。 单击 **"详细信息" > 转到网页上**，或在 Chrome 中，通过选择 "**高级" > 前进到 "[网页]** "，可以绕过此警告。 建议仅将自签名证书用于测试环境。

> [!NOTE]
> 这些说明适用于在 Windows Server 上安装桌面体验，而不是在服务器核心安装上安装。 

1. 将[Windows 管理中心下载](https://aka.ms/windowsadmincenter)到你的本地计算机。

2. 与 VM 建立远程桌面连接，然后从本地计算机复制 MSI，并将其粘贴到 VM 中。

3. 双击 MSI 开始安装，然后按照向导中的说明进行操作。 请注意以下事项：

   - 默认情况下，安装程序使用推荐的端口443（HTTPS）。 如果要选择其他端口，请注意，还需要在防火墙中打开该端口。 

   - 如果已在 VM 上安装 SSL 证书，请确保选择该选项并输入指纹。

4. 启动 Windows 管理中心服务（运行 C：/Program Files/Windows 管理中心/sme）

[详细了解如何部署 Windows 管理中心。](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>配置网关 VM 以启用 HTTPS 端口访问： 

1. 在 Azure 门户中导航到 VM，然后选择 "**网络**"。 

2. 选择 "**添加入站端口规则**"，然后在 "**服务**" 下选择**HTTPS** 。 

> [!NOTE]
> 如果选择默认值443以外的端口，请选择 "服务" 下的 "**自定义**"，然后在 "**端口范围**" 下输入在步骤3中选择的端口。 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>访问安装在 Azure VM 上的 Windows 管理中心网关

此时，你应该能够通过导航到你的网关 VM 的 DNS 名称，从本地计算机上的新式浏览器（边缘或 Chrome）访问 Windows 管理中心。 

> [!NOTE]
> 如果选择了443以外的端口，则可以通过导航到 VM\<\>的 https://DNS 名称来访问 Windows 管理中心：\<自定义端口\>

当你尝试访问 Windows 管理中心时，浏览器将提示你提供凭据，以便访问安装了 Windows 管理中心的虚拟机。 此处需要输入虚拟机的 "本地用户" 或 "本地管理员" 组中的凭据。 

若要在 VNet 中添加其他 Vm，请通过在 PowerShell 中运行以下命令，或在目标 VM 上的命令提示符下运行 WinRM，确保 WinRM 在目标 Vm 上运行：`winrm quickconfig`

如果尚未将 Azure VM 加入域，则 VM 的行为类似于工作组中的服务器，因此需要确保考虑[在工作组中使用 Windows 管理中心](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。