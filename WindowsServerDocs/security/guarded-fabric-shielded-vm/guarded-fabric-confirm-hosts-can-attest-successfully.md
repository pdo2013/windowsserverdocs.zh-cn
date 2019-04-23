---
title: 确认受保护的主机可以证明
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: 6b67208176b426f52d3c5106f8de09ad334d3b01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829528"
---
# <a name="confirm-guarded-hosts-can-attest"></a>确认受保护的主机可以证明 

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016


构造管理员需要用来确认 HYPER-V 主机可以作为受保护的主机运行。 完成至少一个受保护的主机上执行以下步骤：

1.  如果尚未安装 HYPER-V 角色和主机保护者 HYPER-V 支持功能，请使用以下命令安装它们：

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  请确保 HYPER-V 主机可以将 HGS DNS 名称解析和 HGS 服务器上具有网络连接，以访问端口 80 （或如果设置了 HTTPS 的 443）。

2.  配置主机的密钥保护和证明 Url:

    - **通过 Windows PowerShell**:可以通过在提升的 Windows PowerShell 控制台中执行以下命令配置的密钥保护和证明 Url。 为&lt;FQDN&gt;，使用完全限定域 (FQDN) 的 HGS 群集 (例如，hgs.bastion.local，或者向 HGS 管理员运行**Get HgsServer**要检索的 HGS 服务器上的 cmdletUrl)。

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        若要配置回退 HGS 服务器，重复此命令，并指定的密钥保护和证明服务的回退 Url。 有关详细信息，请参阅[回退配置](guarded-fabric-manage-branch-office.md#fallback-configuration)。 

    - **通过 VMM**:如果使用的 System Center 2016-Virtual Machine Manager (VMM) 中，您可以在 VMM 中配置证明和密钥保护 Url。 有关详细信息，请参阅[配置全局 HGS 设置](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings)中**预配受保护的主机在 VMM 中的**。
    
    >**注意**
    > - 如果 HGS 管理员[HGS 服务器上启用 HTTPS](guarded-fabric-configure-hgs-https.md)，开始使用 Url `https://`。
    > - 如果 HGS 管理员 HGS 服务器上启用 HTTPS，并使用自签名的证书，需要将证书导入每个主机上受信任的根证书颁发机构存储区。 若要执行此操作，每个主机上运行以下命令：<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  若要启动主机上的证明尝试并查看证明状态，请运行以下命令：

        Get-HgsClientConfiguration

    命令的输出指示主机是否通过证明，并且现在受保护。 如果`IsHostGuarded`不会返回**True**，可以运行 HGS 诊断工具[Get HgsTrace](https://technet.microsoft.com/library/mt718831.aspx)、 进行调查。 若要运行诊断，请在主机上在提升的 Windows PowerShell 提示符中输入以下命令：

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > 如果您使用的 Windows Server 2019 或 Windows 10，版本 1809年并且正在使用代码完整性策略`Get-HgsTrace`可能返回的失败**代码完整性策略 Active**诊断。
    > 仅故障诊断时，可以放心地忽略此结果。

## <a name="next-step"></a>下一步

>[!div class="nextstepaction"]
[部署受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>请参阅

- [部署主机保护者服务 (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [部署受防护的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

