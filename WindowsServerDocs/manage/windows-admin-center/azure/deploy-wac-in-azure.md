---
title: 部署在 Azure 中的 Windows Admin Center 网关
description: 如何部署在 Azure 中的 Windows Admin Center 网关
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a3fa1838096d910505faf9a2c5bd819b3a256fe2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280412"
---
# <a name="deploy-windows-admin-center-in-azure"></a>部署在 Azure 中 Windows Admin Center

## <a name="deploy-using-script"></a>使用脚本进行部署

您可以下载[部署 WACAzVM.ps1](https://aka.ms/deploy-wacazvm)将从运行[Azure Cloud Shell](https://shell.azure.com)设置在 Azure 中的 Windows Admin Center 网关。 此脚本可以创建整个环境，包括资源组。

[跳转到手动部署步骤](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>先决条件

* 设置你的帐户[Azure Cloud Shell](https://shell.azure.com)。 如果这是你首次使用 Cloud Shell，您将要求您可以关联或使用 Cloud Shell 中创建 Azure 存储帐户。
* 在中**PowerShell** Cloud Shell 中，导航到主目录： ```PS Azure:\> cd ~```
* 若要上传```Deploy-WACAzVM.ps1```文件中，拖放它从本地计算机到任意位置在 Cloud Shell 窗口。

如果指定你自己的证书：

* 将证书上载到[Azure 密钥保管库](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)。 首先，在 Azure 门户中创建密钥保管库，然后将证书上传到密钥保管库。 或者，可以使用 Azure 门户为你生成证书。

### <a name="script-parameters"></a>脚本参数

* **ResourceGroupName** -[String] 指定将在其中创建 VM 的资源组的名称。

* **名称**-[String] 指定的 VM 的名称。

* **凭据**-[PSCredential] 为 VM 指定的凭据。

* **MsiPath** -部署的现有 VM 上的 Windows Admin Center 时，[String] 指定 Windows Admin Center MSI 的本地路径。 默认为从版本 http://aka.ms/WACDownload 如果省略。

* **VaultName** -[String] 指定包含证书的密钥保管库的名称。

* **CertName** -[String] 指定要使用 MSI 安装的证书的名称。

* **GenerateSslCert** -[开关] 如果 MSI 应生成自签名的 ssl 证书，则为 True。

* **端口号**-[int] 指定 Windows Admin Center 服务的 ssl 端口号。 默认值为 443，如果省略。

* **OpenPorts** -[int []] 指定的 vm 打开端口。

* **位置**-[String] 指定 VM 的位置。

* **大小**-[String] 指定的 VM 的大小。 默认值为"Standard_DS1_v2"; 如果省略。

* **映像**-[String] 指定 VM 的映像。 默认值为"Win2016Datacenter"; 如果省略。

* **VirtualNetworkName** -[String] 为 VM 指定的虚拟网络的名称。

* **SubnetName** -[String] 为 VM 指定的子网的名称。

* **SecurityGroupName** -[String] 为 VM 指定的安全组的名称。

* **PublicIpAddressName** -[String] 为 VM 指定的公共 IP 地址的名称。

* **InstallWACOnly** -如果 WAC 应安装在预先存在的 Azure VM 上 [开关] 设置为 True。

有 2 种不同方法的 MSI 部署和使用 MSI 安装的证书。 也可以从 aka.ms/WACDownload 下载 MSI 或者，如果部署到现有 VM，可以提供本地 VM MSI 的文件路径。 可以在任一 Azure 密钥保管库中找到证书或自签名的证书将生成的 MSI。

### <a name="script-examples"></a>脚本示例

首先，定义通用脚本的参数所需的变量。

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>示例 1：使用脚本来部署新的虚拟网络和资源组中的新 VM 上的 WAC 网关。 使用 MSI 从 aka.ms/WACDownload 和 MSI 从自签名的证书。

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>示例 2：#1，但使用 Azure Key Vault 中的证书相同。

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>示例 3：在现有 VM 上使用本地 MSI 部署 WAC。

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>运行 Windows Admin Center 网关的虚拟机的要求

必须打开端口 443 (HTTPS)。
使用脚本为定义的相同变量，您可以使用下面的代码 Azure Cloud Shell 中更新的网络安全组：

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>托管 Azure VM 要求

为端口 5985 (WinRM 通过 HTTP) 必须处于打开状态，并具有活动的侦听器。
可以使用 Azure Cloud Shell 中的以下代码更新管理的节点。 ```$ResourceGroupName``` 并```$Name```作为部署脚本中，使用相同的变量，但将需要使用```$Credential```特定于 VM 正在管理。

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>在现有 Azure 虚拟机上手动部署

之前你所需的网关 VM 上安装 Windows Admin Center，安装要用于 HTTPS 通信的 SSL 证书，或您可以选择使用 Windows Admin Center 生成的自签名的证书。 但是，您将获得一条警告时尝试从浏览器连接，如果您选择后一种选择。 可以通过单击此警告在 Edge 中的绕过**详细信息 > 转到网页**或在 Chrome 中，通过选择**高级 > 转到 [网页]** 。 我们建议仅用于测试环境中使用自签名的证书。

> [!NOTE]
> 这些说明适用于带桌面体验的 Windows Server 上安装不在服务器核心安装上。 

1. [下载 Windows Admin Center](https://aka.ms/windowsadmincenter)到本地计算机。

2. 建立远程桌面连接到 VM，则将 MSI 从本地计算机复制并粘贴到 VM。

3. 双击 MSI 文件开始安装，然后按照向导中的说明。 请注意以下事项：

   - 默认情况下，安装程序使用建议的端口 443 (HTTPS)。 如果你想要选择不同的端口，请注意，你必须也在防火墙中打开该端口。 

   - 如果你已在 VM 上安装 SSL 证书，请确保选择该选项并输入的指纹。

4. 启动 Windows Admin Center 服务 （运行 c: / Program 文件/Windows 管理员 Center/sme.exe）

[了解有关部署 Windows Admin Center 的详细信息。](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>配置网关 VM，以启用 HTTPS 端口访问： 

1. 导航到门户并选择 Azure 中的 VM**网络**。 

2. 选择**添加入站的端口规则**，然后选择**HTTPS**下**服务**。 

> [!NOTE]
> 如果你选择非默认值 443 的端口，选择**自定义**服务下，输入在步骤 3 中选择的端口**端口范围**。 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>访问 Azure VM 上安装 Windows Admin Center 网关

此时，您应能够访问 Windows Admin Center （Edge 或 Chrome） 的新式浏览器从本地计算机上通过导航到你的网关 VM 的 DNS 名称。 

> [!NOTE]
> 如果选择了非 443 的端口，则可以通过导航到 https:// 访问 Windows Admin Center\<VM 的 DNS 名称\>:\<自定义端口\>

当您尝试访问 Windows Admin Center 时，在浏览器将提示输入凭据以访问虚拟机在其安装 Windows Admin Center。 此处需要输入中的本地用户或虚拟机的本地管理员组的凭据。 

若要在 VNet 中添加其他 Vm，请确保 WinRM 目标 Vm 上运行 PowerShell 或命令提示符中运行以下命令在目标 VM 上： `winrm quickconfig`

如果你尚未加入域的 Azure VM，VM 类似于服务器在工作组中，因此将需要确保你考虑[在工作组中使用 Windows Admin Center](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)。