---
title: 包含服务主体名称 (SPN) 的 Kerberos
description: 网络控制器支持多种身份验证方法与管理客户端通信。 您可以使用基于 Kerberos 的身份验证和基于 X509 证书的身份验证。 你还可以选择对测试部署不使用身份验证。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 78d5d2144e0def8e69a2a4ae5fdc2d7718936710
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355766"
---
# <a name="kerberos-with-service-principal-name-spn"></a>包含服务主体名称 (SPN) 的 Kerberos

>适用于：Windows Server 2019

网络控制器支持多种身份验证方法与管理客户端通信。 您可以使用基于 Kerberos 的身份验证和基于 X509 证书的身份验证。 你还可以选择对测试部署不使用身份验证。

System Center Virtual Machine Manager 使用基于 Kerberos 的身份验证。 如果你使用的是基于 Kerberos 的身份验证，则必须在 Active Directory 中为网络控制器配置服务主体名称（SPN）。 SPN 是网络控制器服务实例的唯一标识符，Kerberos 身份验证使用该标识符将服务实例与服务登录帐户关联。 有关更多详细信息，请参阅[服务主体名称](https://docs.microsoft.com/windows/desktop/ad/service-principal-names)。

## <a name="configure-service-principal-names-spn"></a>配置服务主体名称（SPN）

网络控制器会自动配置 SPN。 只需为网络控制器计算机提供注册和修改 SPN 的权限即可。

1.  在域控制器计算机上，开始**Active Directory 用户和计算机**。

2.  选择 **" \>查看高级**"。

3.  在 "**计算机**" 下，找到其中一个网络控制器计算机帐户，然后右键单击并选择 "**属性**"。

4.  选择 "**安全**" 选项卡，然后单击 "**高级**"。

5.  在列表中，如果所有网络控制器计算机帐户或具有所有网络控制器计算机帐户的安全组未列出，请单击 "**添加**" 以添加它。

6.  对于每个网络控制器计算机帐户或包含网络控制器计算机帐户的单个安全组：

    a.  选择帐户或组，然后单击 "**编辑**"。

    b.  在 "权限" 下，选择 "**验证写入 servicePrincipalName**"。

    d.  向下滚动，然后在 "**属性**" 下选择：

       -  **读取 servicePrincipalName**

       -  **写入 servicePrincipalName**

    e.  单击“确定”两次 。

7.  为每个网络控制器计算机重复步骤 3-6。

8.  关闭“Active Directory 用户和计算机”。

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>未能提供 SPN 注册/修改的权限

在**新**的 Windows Server 2019 部署上，如果为 REST 客户端身份验证选择了 Kerberos，但没有为网络控制器节点授予注册或修改 SPN 的权限，则网络控制器上的 REST 操作将失败，从而无法管理SDN。

对于从 Windows Server 2016 到 Windows Server 2019 的升级，并为 REST 客户端身份验证选择了 Kerberos，则不会阻止 REST 操作，从而确保现有生产部署的透明度。 

如果 SPN 未注册，REST 客户端身份验证将使用 NTLM，这不太安全。 还会在**NetworkController**事件通道的管理通道中获取关键事件，要求你向网络控制器节点提供权限来注册 SPN。 提供权限后，网络控制器会自动注册 SPN，所有客户端操作都将使用 Kerberos。


>[!TIP]
>通常，可以将网络控制器配置为使用 IP 地址或 DNS 名称进行基于 REST 的操作。 但是，配置 Kerberos 时，不能将 REST 查询的 IP 地址用于网络控制器。 例如\<，你可以使用 https://networkcontroller.consotso.com\> ，但不能使用\< https://192.34.21.3\> 。 如果使用 IP 地址，则服务主体名称不起作用。
>
>如果在 Windows Server 2016 中将用于 REST 操作的 IP 地址与 Kerberos 身份验证一起使用，则实际通信将经过 NTLM 身份验证。 在这种部署中，一旦升级到 Windows Server 2019，你将继续使用基于 NTLM 的身份验证。 若要移动到基于 Kerberos 的身份验证，必须使用网络控制器 DNS 名称进行 REST 操作，并为网络控制器节点提供注册 SPN 的权限。

---