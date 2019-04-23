---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: 事件 ID 2088-DNS 查找失败，出现了复制成功
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: cc090fa749a601e53b4347cce43245f22badc8ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840708"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件 ID 2088:DNS 查找失败，出现了复制成功

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

无效的 DNS 配置可能会影响成员计算机、 域控制器或在此 Active Directory 域服务林中，包括登录身份验证或访问网络资源的应用程序服务器上的其他基本操作。 

以使此域控制器可以解析的源域控制器使用 DNS 的 IP 地址，应尽快解决此 DNS 配置错误。 

备用服务器名称：DC1 失败的 DNS 主机名：4a8717eb-8e58-456c-995a-c92e4add7e8e._msdcs.contoso.com 

注意：默认情况下，只有最多 10 个 DNS 失败显示对于任何给定的 12 小时期间，即使出现超过 10 个故障。  若要记录单个失败的所有事件，以下诊断注册表值设置为 1: 

注册表路径：HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

用户执行任何操作： 

1) 如果源域控制器不再正常运行或其操作系统重新安装具有不同的计算机名称或 NTDSDSA 对象 GUID，删除使用 ntdsutil.exe，源域控制器的元数据已参阅 MSKB 文章中所述的步骤216498。 

2) 确认源域控制器正在运行 Active Directory，且可访问网络上通过键入"net 视图\\&lt;源 DC 名称&gt;"或"ping&lt;源 DC 名称&gt;"。 

3) 验证源域控制器使用有效的 DNS 服务器的 DNS 服务和源域控制器的主机记录和 CNAME 记录是正确注册，使用 DCDIAG 的 DNS 增强版本。EXE 上可用 https://www.microsoft.com/dns 

dcdiag /test:dns 

4) 验证此目标域控制器使用有效的 DNS 服务器的 DNS 服务，通过运行 DCDIAG 的 DNS 增强版本。在控制台上的目标域控制器，如下所示的 EXE 命令： 

dcdiag /test:dns 

5) 以便进行进一步分析 DNS 错误失败，请参阅 KB 824449: https://support.microsoft.com/?kbid=824449 

其他数据错误值：11004 请求的名称是有效的但未找到所请求类型的任何数据</code> </introduction>
  <section>
    <title>诊断</title>
    <content>
      <para>无法解析在 DNS 中使用别名 (CNAME) 资源记录的源域控制器名称可能是由于 DNS 配置错误或在 DNS 数据传播延迟。</para>
    </content>
  </section>
  <section>
    <title>解决方法</title>
    <content>
      <para>继续进行 DNS 测试中所述"<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">事件 ID 2087:DNS 查找失败导致复制失败</link>。"</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


