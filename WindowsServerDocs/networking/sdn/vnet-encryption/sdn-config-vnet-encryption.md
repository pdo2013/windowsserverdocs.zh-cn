---
title: 为虚拟网络配置加密
description: 虚拟网络加密允许加密虚拟机之间的虚拟网络流量，这些虚拟机在标记为 "已启用加密" 的子网中相互通信。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: ebd086c6fa93f7ab5855debfd1f71845fb9ff309
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870058"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>为虚拟子网配置加密

>适用于：Windows Server

虚拟网络加密允许对在标记为 "已启用加密" 的子网中相互通信的 Vm 之间的虚拟网络通信进行加密。 它还利用虚拟子网上的数据报传输层安全性 (DTLS) 来加密数据包。 DTLS 可以防止能够访问物理网络的任何人进行窃听、篡改和伪造。

虚拟网络加密要求：
- 在每个启用了 SDN 的 Hyper-v 主机上安装的加密证书。
- 网络控制器中引用该证书的指纹的凭据对象。
- 每个虚拟网络上的配置都包含需要加密的子网。

在子网中启用加密后，该子网中的所有网络流量都将自动进行加密，以及任何可能发生的应用程序级加密。  跨子网的流量（即使标记为已加密）将自动以未加密的方式发送。 跨虚拟网络边界的任何流量也会以未加密状态发送。

>[!NOTE]
>与同一子网上的其他 VM 通信时，无论其当前是否已连接或已连接，流量都会自动加密。

>[!TIP]
>如果你必须将应用程序限制为仅在加密的子网上进行通信，则只能使用访问控制列表（Acl）来允许当前子网中的通信。 有关详细信息，请参阅[使用访问控制列表（acl）管理数据中心网络流量流](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。


## <a name="step-1-create-the-encryption-certificate"></a>步骤 1： 创建加密证书
每个主机都必须安装加密证书。 可以对所有租户使用相同的证书，也可以为每个租户生成唯一的证书。 

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

运行该脚本后，"我的存储" 中将出现一个新证书：

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2. 将证书导出到文件。<p>需要证书的两个副本，一个具有私钥，另一个不包含。

```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
```

3. 在每个 hyper-v 主机上安装证书 

   PS c：\> dir c：\$subjectname. *


~~~
    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
-a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx
~~~

4. 在 Hyper-v 主机上安装

```
   $server = "Server01"

   $subjectname = "EncryptedVirtualNetworks"
   copy c:\$SubjectName.* \\$server\c$
   invoke-command -computername $server -ArgumentList $subjectname,"secret" {
       param (
           [string] $SubjectName,
           [string] $Secret
       )
       $certFullPath = "c:\$SubjectName.cer"

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
```

5. 为环境中的每个服务器重复此步骤。<p>为每个服务器重复后，你应该在每个 Hyper-v 主机的根目录和 my 存储中安装证书。 

6. 验证证书的安装。<p>通过检查 My 和 Root 证书存储的内容来验证证书：

   PS C：\> enter-pssession Server1

~~~
[Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks
~~~

7. 记下指纹。<p>必须记下指纹，因为你需要它在网络控制器中创建证书凭据对象。

## <a name="step-2-create-the-certificate-credential"></a>步骤 2： 创建证书凭据

在连接到网络控制器的每个 Hyper-v 主机上安装证书后，你现在必须将网络控制器配置为使用该证书。  为此，必须从安装了网络控制器 PowerShell 模块的计算机创建包含证书指纹的凭据对象。 

```
    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  

    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller

    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force
```
>[!TIP]
>你可以为每个加密的虚拟网络重复使用此凭据，也可以为每个租户部署并使用唯一的证书。


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>步骤 3： 配置虚拟网络以进行加密

此步骤假定你已创建虚拟网络名称 "我的网络" 并且包含至少一个虚拟子网。  有关创建虚拟网络的信息，请参阅[创建、删除或更新租户虚拟网络](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md)。

>[!NOTE]
>与同一子网上的其他 VM 通信时，无论其当前是否已连接或已连接，流量都会自动加密。

1.  从网络控制器中检索虚拟网络和凭据对象
```
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"
```
2.  添加对证书凭据的引用并在单个子网上启用加密
```
    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true
```
3.  将已更新的虚拟网络对象放入网络控制器
```
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force
```

_**恭喜!**_ 完成这些步骤后，你就完成了。 


## <a name="next-steps"></a>后续步骤



