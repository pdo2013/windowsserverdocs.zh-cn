---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: "事件 ID 2088-发生复制成功 DNS 查阅故障"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4f223f075775f942f83a1962da28a77e85e89aa0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>事件 ID 2088：复制成功出现了 DNS 查询失败

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

    
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

无效 DNS 配置可能会影响的其他成员计算机、域控制器或这包括登录身份验证或访问网络资源的 Active Directory 域服务片森林中的应用程序服务器上的基本操作。 

你应立即解决此 DNS 配置错误，以便此域控制器可以解决使用 DNS 源域控制器的 IP 地址。 

备用服务器名称：DC1 无法 DNS 主机名称：4a8717eb-8e58 456 c-995a-c92e4add7e8e._msdcs.contoso.com 

注意：默认情况下，仅最多 10 DNS 失败显示任何给定 12 小时时间，即使出现超过 10 故障。  若要登录所有个别故障事件，以下诊断注册表值设置为 1: 

注册表路径：HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC 客户端 

用户操作： 

1) 如果不再源域控制器已重新正常工作或其操作系统安装具有不同的计算机名称或 NTDSDSA 对象 GUID，删除源域控制器元数据的 ntdsutil.exe，使用已参阅 MSKB 文章 216498 中所述的步骤。 

2) 确认源域控制器在运行 Active Directory，并通过键入是网络上可以访问"网视图 \\&lt;源直流名称&gt;"或"ping&lt;源直流名称&gt;"。 

3) 验证对 DNS 服务和录制源域控制器主机记录并 CNAME 是，使用源域控制器 DNS 服务器是否有效正确注册，使用 DCDIAG.EXE 适用于 https://www.microsoft.com/dns 

dcdiag//test: dns 

4) 确认目标该域控制器使用 DNS 服务器是否有效 DNS 服务通过运行 DCDIAG.EXE 命令目标域控制器，在主机上，如下所示： 

dcdiag//test: dns 

5) 有关进一步分析 DNS 错误失败，请参阅 KB 824449: https://support.microsoft.com/?kbid=824449 

其他数据错误值：11004 请求的名称有效，但没有数据的请求类型找</code>
  </introduction>
  <section>
    <title>诊断</title>
    <content>
      <para>但无法通过在 DNS (CNAME) 别名资源记录解决源控制器域名可能因 DNS 错误配置或 DNS 数据传播中的延迟。</para>
    </content>
  </section>
  <section>
    <title>分辨率</title>
    <content>
      <para>继续 DNS 测试，述"<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">事件 ID 2087: DNS 查询失败导致复制失败</link>。"</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


