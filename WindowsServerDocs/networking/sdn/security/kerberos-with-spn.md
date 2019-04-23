---
title: 包含服务主体名称 (SPN) 的 Kerberos
description: 网络控制器支持多种身份验证方法与管理客户端进行通信。 可以使用基于 Kerberos 身份验证，X509 基于证书的身份验证。 此外可以选择要用于测试部署无身份验证。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2459cfa8dfec3de4aa23da7aba192d6eeed7f8ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828968"
---
# <a name="kerberos-with-service-principal-name-spn"></a>包含服务主体名称 (SPN) 的 Kerberos

>适用于：Windows Server 2019

网络控制器支持多种身份验证方法与管理客户端进行通信。 可以使用基于 Kerberos 身份验证，X509 基于证书的身份验证。 此外可以选择要用于测试部署无身份验证。

System Center Virtual Machine Manager 使用基于 Kerberos 的身份验证。 如果使用基于 Kerberos 的身份验证，您必须在 Active Directory 中的网络控制器的配置服务主体名称 (SPN)。 SPN 是 Kerberos 身份验证用于将服务实例相关联的服务登录帐户与网络控制器服务实例的唯一标识符。 有关更多详细信息，请参阅[服务主体名称](https://docs.microsoft.com/windows/desktop/ad/service-principal-names)。

## <a name="configure-service-principal-names-spn"></a>配置服务主体名称 (SPN)

网络控制器会自动配置 SPN。 您需要做是提供注册和修改 SPN 的网络控制器计算机的权限。

1.  在域控制器计算机上启动**Active Directory 用户和计算机**。

2.  选择**视图\>高级**。

3.  下**计算机**，找到一个网络控制器计算机帐户，然后右键单击并选择**属性**。

4.  选择**安全**选项卡，单击**高级**。

5.  在列表中，如果所有网络控制器计算机帐户或安全组具有未列出所有网络控制器计算机帐户，请单击**添加**以将其添加。

6.  对于每个网络控制器计算机帐户或单独的安全组包含网络控制器计算机帐户：

    a.  选择帐户或组，然后单击**编辑**。

    b.  在权限选择下**验证写入 servicePrincipalName**。

    d.  向下的滚动并下**属性**选择：

       -  **读取 servicePrincipalName**

       -  **写入 servicePrincipalName**

    e.  单击“确定”两次  。

7.  为每个网络控制器机重复步骤 3-6。

8.  关闭“Active Directory 用户和计算机” 。

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>如果未提供 SPN 注册/修改的权限

上**新建**Windows Server 2019 部署，如果您选择 Kerberos REST 客户端身份验证，且不授予网络控制器节点注册或修改 SPN 的权限，在网络控制器上的 REST 操作失败使你无法管理 SDN。

有关从 Windows Server 2016 升级到 Windows Server 2019，REST 客户端身份验证选择 Kerberos，REST 操作不会阻止，确保对于现有的生产部署的透明度。 

如果未注册 SPN，REST 客户端身份验证使用 NTLM，这是不太安全。 还有关键事件中的管理通道**NetworkController Framework**事件通道要求你提供到网络控制器节点的权限注册 SPN。 一旦你提供的权限，网络控制器自动注册的 SPN 和客户端的所有操作都使用 Kerberos。


>[!TIP]
>通常情况下，可以配置网络控制器使用的 IP 地址或 DNS 名称的基于 REST 的操作。 但是，当您配置 Kerberos 时，不能用于 IP 地址到网络控制器 REST 查询。 例如，可以使用\< https://networkcontroller.consotso.com\>，但不能使用\< https://192.34.21.3\>。 如果使用的 IP 地址无法正常运行的服务主体名称。
>
>如果所使用的 REST 操作，以及 Windows Server 2016 中的 Kerberos 身份验证的 IP 地址的实际通信就已通过 NTLM 身份验证。 在此类部署中，一旦升级到 Windows Server 2019 您继续使用基于 NTLM 的身份验证。 若要移动到基于 Kerberos 的身份验证，必须使用适用于 REST 操作的网络控制器 DNS 名称，并提供用于网络控制器节点注册 SPN 的权限。

---