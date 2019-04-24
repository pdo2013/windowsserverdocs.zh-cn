---
title: 配置虚拟网络的加密
description: 虚拟网络加密允许加密标记为启用加密。 的子网中相互通信的虚拟机之间的虚拟网络流量
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 90fb33eb4c4b63fdd5c84bf3ffc2447fd52a809b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845488"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>为虚拟子网配置加密

>适用于：Windows Server

虚拟网络加密允许加密的 Vm 标记为启用加密。 的子网中相互之间的虚拟网络流量 它还利用虚拟子网上的数据报传输层安全性 (DTLS) 来加密数据包。 DTLS 可以防止能够访问物理网络的任何人进行窃听、篡改和伪造。

虚拟网络加密要求：
- 每个已启用 SDN 的 HYPER-V 主机上安装的加密证书。
- 引用该证书的指纹在网络控制器中的凭据对象。
- 每个虚拟网络上的配置包含需要加密的子网。

一旦启用的子网上的加密，该子网中的所有网络流量除了可能也会发生任何应用程序级别加密自动都加密。  流量，即使标记为已加密，跨子网，之间将自动发送未加密。 跨虚拟网络边界的任何流量也发送未加密。

>[!NOTE]
>当与同一子网上的另一个 VM 进行通信，无论其当前已连接或在更高版本时，流量已连接获取自动加密。

>[!TIP]
>如果必须限制应用程序仅上进行通信的加密的子网，可以使用访问控制列表 (Acl) 仅以允许在当前的子网内的通信。 有关详细信息，请参阅[使用访问控制列表 (Acl) 管理数据中心网络流量流到](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。


## <a name="step-1-create-the-encryption-certificate"></a>步骤 1： 创建加密证书
每个主机必须安装加密证书。 可以为所有租户使用相同的证书，也可以生成一个唯一的每个租户。 

1.  生成证书  

```
    $subjectName = "EncryptedVirtualNetworks"
    $cryptographicProviderName = "Microsoft Base Cryptographic Provider v1.0";
    [int] $privateKeyLength = 1024;
    $sslServerOidString = "1.3.6.1.5.5.7.3.1";
    $sslClientOidString = "1.3.6.1.5.5.7.3.2";
    [int] $validityPeriodInYear = 5;

    $name = new-object -com "X509Enrollment.CX500DistinguishedName.1"
    $name.Encode("CN=" + $SubjectName, 0)

    #Generate Key
    $key = new-object -com "X509Enrollment.CX509PrivateKey.1"
    $key.ProviderName = $cryptographicProviderName
    $key.KeySpec = 1 #X509KeySpec.XCN_AT_KEYEXCHANGE
    $key.Length = $privateKeyLength
    $key.MachineContext = 1
    $key.ExportPolicy = 0x2 #X509PrivateKeyExportFlags.XCN_NCRYPT_ALLOW_EXPORT_FLAG 
    $key.Create()

    #Configure Eku
    $serverauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $serverauthoid.InitializeFromValue($sslServerOidString)
    $clientauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $clientauthoid.InitializeFromValue($sslClientOidString)
    $ekuoids = new-object -com "X509Enrollment.CObjectIds.1"
    $ekuoids.add($serverauthoid)
    $ekuoids.add($clientauthoid)
    $ekuext = new-object -com "X509Enrollment.CX509ExtensionEnhancedKeyUsage.1"
    $ekuext.InitializeEncode($ekuoids)

    # Set the hash algorithm to sha512 instead of the default sha1
    $hashAlgorithmObject = New-Object -ComObject X509Enrollment.CObjectId
    $hashAlgorithmObject.InitializeFromAlgorithmName( $ObjectIdGroupId.XCN_CRYPT_HASH_ALG_OID_GROUP_ID, $ObjectIdPublicKeyFlags.XCN_CRYPT_OID_INFO_PUBKEY_ANY, $AlgorithmFlags.AlgorithmFlagsNone, "SHA512")


    #Request Certificate
    $cert = new-object -com "X509Enrollment.CX509CertificateRequestCertificate.1"

    $cert.InitializeFromPrivateKey(2, $key, "")
    $cert.Subject = $name
    $cert.Issuer = $cert.Subject
    $cert.NotBefore = (get-date).ToUniversalTime()
    $cert.NotAfter = $cert.NotBefore.AddYears($validityPeriodInYear);
    $cert.X509Extensions.Add($ekuext)
    $cert.HashAlgorithm = $hashAlgorithmObject
    $cert.Encode()

    $enrollment = new-object -com "X509Enrollment.CX509Enrollment.1"
    $enrollment.InitializeFromRequest($cert)
    $certdata = $enrollment.CreateRequest(0)
    $enrollment.InstallResponse(2, $certdata, 0, "")
```

运行脚本后，新的证书将出现在我的存储：

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2.  将证书导出到文件。<p>所需证书，一个包含私钥，另一个不包含两个的副本。

    $subjectName = "EncryptedVirtualNetworks" $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"} [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret")) Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

3.  每个 hyper-v 主机上安装证书 

    PS c:\> dir c:\$subjectname.*


        Directory: C:\


    模式上次写入时间长度名称
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx

4.  在 HYPER-V 主机上安装

    $server = "Server01"

    $subjectname ="EncryptedVirtualNetworks"复制 c:\$SubjectName.* \\$server\c$ 调用命令-computername $server-ArgumentList $subjectname，"机密"{param ([string] $SubjectName，[string] $Secret) $certFullPath ="c:\$SubjectName.cer"

        # create a representation of the certificate file
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath)

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        $certFullPath = "c:\$SubjectName.pfx"
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath, $Secret, "MachineKeySet,PersistKeySet")

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("My", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        # Important: Remove the certificate files when finished
        remove-item C:\$SubjectName.cer
        remove-item C:\$SubjectName.pfx
    }    

5.  你的环境中每个服务器重复。<p>后重复的每个服务器，你应该具有根目录和我的每个 HYPER-V 主机的存储中安装的证书。 

6.  验证安装了该证书。<p>通过检查的内容来验证证书我和根证书存储区：

    PS c:\>输入 pssession Server1

    [Server1]:PS c:\>的 get-childitem cert://localmachine/my、 cert://localmachine/root |？ {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath:Microsoft.PowerShell.Security\Certificate::localmachine\my

    指纹主题
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


    PSParentPath:Microsoft.PowerShell.Security\Certificate::localmachine\root

    指纹主题
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

7.  记下的指纹。<p>因为您需要它来在网络控制器中创建的证书凭据对象，你必须记下的指纹。

## <a name="step-2-create-the-certificate-credential"></a>步骤 2： 创建证书凭据

每个连接到网络控制器的 HYPER-V 主机上安装证书后，现在必须配置网络控制器可以使用它。  若要执行此操作，必须创建一个凭据对象，与安装的网络控制器 PowerShell 模块包含从计算机的证书指纹。 


    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

>[!TIP]
>可以重复使用此凭据对于每个加密的虚拟网络，也可以部署并为每个租户使用唯一的证书。


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>步骤 3： 为加密配置虚拟网络

此步骤假定你已创建虚拟网络名称"我的网络"，并且它包含至少一个虚拟子网。  有关创建虚拟网络的信息，请参阅[创建、 删除或更新租户虚拟网络](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md)。

>[!NOTE]
>当与同一子网上的另一个 VM 进行通信，无论其当前已连接或在更高版本时，流量已连接获取自动加密。

1.  从网络控制器中检索的虚拟网络和凭据对象

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork" $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

2.  添加对证书凭据的引用和单个子网上启用加密

    $vnet.properties.EncryptionCredential = $certcred

    # <a name="replace-the-subnets-index-with-the-value-corresponding-to-the-subnet-you-want-encrypted"></a>将与要加密的子网对应的值替换为子网索引。  
    # <a name="repeat-for-each-subnet-where-encryption-is-needed"></a>为需要加密每个子网重复执行
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

3.  将更新的虚拟网络对象放入网络控制器

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force


_**祝贺您 ！**_ 完成后完成这些步骤。 


## <a name="next-steps"></a>后续步骤



