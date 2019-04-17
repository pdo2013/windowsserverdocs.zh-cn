---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: "有复制错误 1753 年有更多端点可从端点映射程序"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e7412f5edc6c206888551fdc250883b5c0ced3e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>有复制错误 1753 年有更多端点可从端点映射程序

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>本主题介绍症状、的原因并解决 Active Directory 复制错误 8524 DSA 操作将无法继续由于 DNS 查询失败。</para>
    <list class="bullet">
      <listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">症状</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">原因</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">分辨率</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">更多信息</link></para></listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>症状</title>
    <content>
      <para>本指南介绍了症状，原因和分辨率步骤 Win32 错误 1753 年失败的 Active Directory 操作:"没有多个端点端点映射程序可用。"</para>
      <list class="ordered">
        <listItem>
          <para>连接测试、Active Directory 复制测试或 KnowsOfRoleHolders 测试失败，错误 1753 年的 DCDIAG 报告:"没有多个端点端点映射程序可用。"</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN。EXE 报告该复制尝试与状态 1753 年失败。</para><para>通常告知 1753 年状态的 REPADMIN 命令包括但不是限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>示例输出，从"REPADMIN /SHOWREPS"描述 CONTOSO DC2 从的入站的复制到 CONTOSO DC1 失败并显示"复制无法访问"错误如下所示：</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para><ui>检查复制拓扑</ui>Active Directory 的站点和服务中的命令返回"没有多个端点端点映射程序可用。"</para><para>来源 DC 连接对象右键单击并选择<ui>检查复制拓扑</ui>失败，"没有多个端点端点映射程序可用。"下方显示的错误消息是屏幕上：</para><para>对话框标题文本：检查复制拓扑</para><para>对话框消息的文本：</para><para>试图联系域控制器期间出现以下错误：有更多端点端点映射程序可用。</para></listItem><listItem><para><ui>立即复制</ui>Active Directory 的站点和服务中的命令返回"没有多个端点端点映射程序可用。"</para><para>来源 DC 连接对象右键单击并选择<ui>立即复制</ui>失败，"没有多个端点端点映射程序可用。"下方显示的错误消息是屏幕上：</para><para>对话框标题文本：复制现在</para><para>对话框消息的文本：尝试同步命名上下文期间出现以下错误&lt;%目录分区名称 %&gt;从域控制器&lt;源 DC&gt;到域控制器&lt;目标直流&gt;:</para><para>

没有可从端点映射的更多端点。</para><para>操作不会继续</para></listItem><listItem><para>NTDS KCC、NTDS 常规或 Microsoft-Windows ActiveDirectory_DomainService 事件-2146893022 状态登录事件查看器中的目录服务日志。</para><para>通常告知-2146893022 状态的 Active Directory 事件包括但不是限于：</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>事件 ID</para></TD><TD><para>事件源</para></TD><TD><para>事件字符串</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS 常规</para></TD><TD><para>尝试使用以下全球目录通信，Active Directory 并尝试未成功。</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>NTDS KCC</para></TD><TD><para>尝试建立复制链接的以下目录可写分区失败。</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>NTDS KCC</para></TD><TD><para>若要添加的以下目录分区和源域控制器复制协议尝试知识一致性检查器 (KCC) 失败。</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>原因</title>
    <content>
      <para>下图显示 RPC 工作流向客户端应用程序在步骤 7 RPC 客户端从数据传递到开始服务器应用程序与 RPC 端点映射程序（企业项目管理）在第 1 步中的注册。</para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>步骤 1 到 7 映射到以下操作：</para>
      <list class="ordered">
        <listItem>
          <para>服务器应用注册其端点与 RPC 端点映射程序（企业项目管理）</para>
        </listItem>
        <listItem>
          <para>客户端进行 RPC 通话（代表用户、操作系统或应用程序启动操作）</para>
        </listItem>
        <listItem>
          <para>客户端侧面 RPC 联系人的目标计算机企业项目管理和寻求来完成客户端呼叫端点</para>
        </listItem>
        <listItem>
          <para>服务器计算机企业项目管理响应与端点</para>
        </listItem>
        <listItem>
          <para>客户端 RPC 联系人服务器应用</para>
        </listItem>
        <listItem>
          <para>服务器应用执行调用为客户端 RPC 返回的结果</para>
        </listItem>
        <listItem>
          <para>客户端 RPC 回客户端应用将结果</para>
        </listItem>
      </list>
      <para>失败 1753 年生成的之间步骤 3 和 4 故障。具体来说，1753 年错误意味着 RPC 客户端（目标 DC）是能够端口 135 联系 RPC 服务器（源 DC），但企业 RPC 服务器（源 DC）上的项目管理找不到 RPC 应用程序的兴趣和返回服务器端错误 1753 年。显示 1753 年错误表明 RPC 客户端（目标 DC）收到从 RPC 服务器（广告复制来源 DC）服务器端错误响应通过该网络。</para>
      <para>1753 年错误的特定的根本原因包括：</para>
      <list class="ordered">
        <listItem>
          <para>永远不会启动服务器应用（即永远不会尝试步骤 1 上方的"的详细信息"图）。</para>
        </listItem>
        <listItem>
          <para>服务器应用启动，但阻止它 RPC 端点映射程序（即第 1 步中"的详细信息"图是尝试而失败）进行注册的初始化期间发生某些失败。</para>
        </listItem>
        <listItem>
          <para>服务器应用启动，但随后死了。（即第 1 步中"的详细信息"图已成功完成，但已撤消稍后因为服务器死了）。</para>
        </listItem>
        <listItem>
          <para>服务器应用手动注销其端点（类似于但有意 3 平板电脑。但不是可能包含的完整性。）</para>
        </listItem>
        <listItem>
          <para>联系到 IP 映射错误 DNS、WINS 或主机/Lmhosts 文件中的一个名称由于比预期不同 RPC 服务器时 RPC 客户端（目标 DC）。</para>
        </listItem>
      </list>
      <para>错误 1753 年不由：</para>
      <list class="bullet">
        <listItem>
          <para>轻端口 135 RPC 客户端（目标 DC）和 RPC 服务器（源 DC）之间的网络连接缺乏</para>
        </listItem>
        <listItem>
          <para>缺乏 RPC 服务器（源 DC）之间的网络连接通过短暂端口中使用 135 端口和 RPC 客户端（目标 DC）。</para>
        </listItem>
        <listItem>
          <para>密码不匹配或通过源 DC 导致无法解密 Kerberos 加密的数据包</para>
        </listItem>
      </list>
      <para></para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>分辨率</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>验证注册端点映射程序与它服务该服务已经启动</embeddedLabel>
          </para>
          <para>For Windows 2000 和 Windows Server 2003 Dc：确保插入正常模式下启动时 DC 的来源。</para>
          <para> Windows Server 2008 或 Windows Server 2008 R2：从源 DC 主机，开始服务管理器 (services.msc)，并验证<embeddedLabel>Active Directory 域服务</embeddedLabel>服务正在运行。</para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>验证 RPC 客户端（目标 DC）连接到预期 RPC 服务器（源 DC）</embeddedLabel>
          </para>
          <para>常见 Active Directory 森林寄存器域控制器 CNAME 录制 _msdcs 中的所有 Dc。&lt;森林根域&gt;DNS 无论驻留内森林哪些域的区域。DC CNAME 记录派生自<embeddedLabel>objectGUID</embeddedLabel>各自的对象每个域控制器属性。</para>
          <para>执行基于复制的操作时，目标 DC 查询源 Dc CNAME 记录 DNS。CNAME 记录包含用于派生源 Dc IP 地址的源 DC 完整的计算机名称通过 DNS 客户端的缓存查找主机 / LMHost 文件查阅，主机 A / DNS 或 WINS AAAA 录制。</para>
          <para>过时各自的对象和 DNS、WINS、主机和 LMHOST 文件中的错误名称-TO-IP 映射可能导致连接到错误 RPC 服务器 (源 DC) RPC 客户端（目标 DC）。此外，TO-IP 映射错误名称-可能导致 RPC 客户端（目标 DC）连接到的计算机，即使没有 RPC 服务器应用的兴趣（在本例中的 Active Directory 角色）安装。(示例：DC2 过时主机记录包含 DC3 或成员计算机的 IP 地址)。</para>
          <para>验证源目标 Dc 副本的 Active Directory 中存在的 DC objectGUID 匹配存储在源 Active Directory Dc 副本源 DC objectGUID。如果有差异，则使用 repadmin /showobjmeta ntds 设置对象查看哪一个对应于源直流的最后一个推广上 (提示：比较日期标记为各自的对象创建日期从 /showobjmeta 针对源 Dc dcpromo.log 文件中的最后一个推广日期。你可能需要使用的最后一个修改 / 创建 DCPROMO.LOG 文件本身。）对象 Guid 均不相同，如果目标 DC 可能有其 CNAME 记录指主机记录具有错误名称 IP 映射到一个过时源 DC 各自的对象。</para>
          <para>DC 目标，在运行 IPCONFIG//ALL 确定哪一个 DNS 服务器目标 DC 使用的名称解析：</para>
          <code>c:&gt;ipconfig /all</code>
          <para>对源 Dc 完全限定 DC CNAME 记录 DC 目标，在运行 NSLOOKUP:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>验证，NSLOOKUP 返回的 IP 地址"拥有"主机名 / 源直流安全身份：</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>或</para>
          <para>登录源 DC 主机从 CMD 提示符运行"IPCONFIG"，然后确认源 DC 拥有 NSLOOKUP 命令上述返回的 IP 地址</para>
          <para>检查 DNS 中 IP 映射到过时 / 重复主机</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>无效的 IP 地址存在，主机记录中，如果调查 DNS 清理已启用并正确配置。</para><para>如果上述测试或网络跟踪未显示名称查询退回无效的 IP 地址，请考虑在主机的文件、LMHOSTS 文件和 WINS 服务器过时条目。请注意，DNS 服务器可能也配置执行 WINS 回退名称分辨率。</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>服务器应用程序（Active Directory 等）具有注册端点映射程序 RPC 服务器（源 DC）上的验证</embeddedLabel>
          </para>
          <para>Active Directory 使用多种普遍且动态注册端口。下表列出了熟知端口和使用 Active Directory 域控制器的协议。</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>RPC 服务器应用程序</para>
                </TD>
                <TD>
                  <para>端口</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>DNS 服务器</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP 服务器</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>LDAP SSL</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>全球目录服务器</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>全球目录服务器</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>端点映射程序与未注册已知端口。</para>
          <para>Active Directory 和其他应用也注册接收动态分配的端口 RPC 暂时端口覆盖范围内的服务。此类 RPC 服务器应用程序动态分配 1024 年和 Windows 2000 和 Windows Server 2003 计算机上的 5000 之间 TCP 端口和 49152 与 65535 范围在 Windows Server 2008 和 Windows Server 2008 R2 计算机之间的端口。使用复制的 RPC 端口可能很难编码使用中记录步骤的注册表中<externalLink><linkText>知识库文章 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>。继续使用企业项目管理时配置为使用努力编码的端口注册 Active Directory。</para>
          <para>验证 RPC 服务器应用程序感兴趣的已注册本身 RPC 端点映射程序 RPC 服务器（源直流就广告复制而言）上。</para>
          <para>有多种方式来完成此任务中，但之一是，安装并源 DC 使用语法控制台上的管理员权限 CMD 提示符下运行 PORTQRY: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>portqry 输出中，注意的动态"MS NT 目录 DRS 接口"通过注册端口号 (UUID = 351...) 的<embeddedLabel>ncacn_ip_tcp 协议</embeddedLabel>。下方显示的示例 portquery 片段输出从 Windows Server 2008 R2 域控制器和 UUID / 协议配对特定使用 Active Directory 突出显示<embeddedLabel>改为粗体</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>可能的其他方法解决此错误：</para>
          <list class="ordered">
            <listItem>
              <para>验证在正常模式下启动时源 DC 和源直流上的操作系统和直流角色已完全开始。</para>
            </listItem>
            <listItem>
              <para>Active Directory 域服务正在运行的验证。如果当前停止或在没有配置启动的默认值服务，重置启动默认值，重新启动修改的直流，然后重试。</para>
            </listItem>
            <listItem>
              <para>验证启动价值和服务状态 RPC 服务和 RPC 定位器是否正确操作系统版本的客户端 RPC（目标 DC）和 RPC 服务器（源 DC）。如果当前停止或在没有配置启动的默认值服务，重置启动默认值，重新启动修改的直流，然后重试。</para>
              <para>此外，请确保服务上下文匹配如下表中所列出的默认设置。</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>服务</para>
                    </TD>
                    <TD>
                      <para>Windows Server 2003 及更高版本中的默认状态（启动键入） </para>
                    </TD>
                    <TD>
                      <para>在 Windows Server 2000 默认状态（启动键入）</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>远程过程调用</para>
                    </TD>
                    <TD>
                      <para>启动（自动）</para>
                    </TD>
                    <TD>
                      <para>启动（自动）</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>远程过程调用定位器</para>
                    </TD>
                    <TD>
                      <para>为空或停止（手动）</para>
                    </TD>
                    <TD>
                      <para>启动（自动）</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>验证动态端口区域的大小不已约束。Windows Server 2008 和 Windows Server 2008 R2 NETSH 语法枚举 RPC 端口范围如下所示：</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>验证定义以 kb 为单位 224196 硬编码的端口定义在动态端口范围内，对于源 Dc 操作系统版本。</para>
              <para>查看<externalLink><linkText>知识库文章 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>，并确保努力编码的端口瀑布暂时端口范围内，源 DC 的操作系统版本。</para>
            </listItem>
            <listItem>
              <para>验证 ClientProtocols 密钥存在下 HKLM\Software\ Microsoft \Rpc，并且包含以下 5 默认值：</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>详细信息</title>
    <content>
      <para>
        <embeddedLabel>到 IP 映射导致 RPC 错误 1753 年-2146893022 与坏名称的示例：不正确的目标主要名称</embeddedLabel>
      </para>
      <para>contoso.com 域包含 DC1 以及 DC2 ip 地址 x.x.1.1 和 x.x.1.2。主机"A"/"AAAA"记录 DC2 正确注册所有配置 DC1 DNS 服务器上。此外，在 DC1 主机文件包含映射到 IP 地址 x.x.1.2 DC2s 完全合格主机的项目。更高版本，DC2 的 IP 地址更改 X.X.1.2 从 X.X.1.3 和一台新成员电脑已加入域的 IP 地址 x.x.1.2。广告复制由触发尝试<ui>立即复制</ui>Active Directory 的站点和服务管理单元中的命令将失败，错误 1753 年，下面的跟踪中所示：</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>在帧<embeddedLabel>10</embeddedLabel>，目标 DC 端口 135 Active Directory 复制服务类 UUID E351 查询源 Dc 终止点映射…</para>
      <para>框架中<embeddedLabel>11</embeddedLabel>，源 DC，在此情况下，不能主动 DC 角色，因此尚未注册 E351 成员计算机…使用其当地的企业项目管理复制服务 UUID 响应符号错误 EP_S_NOT_REGISTERED 将映射到小数点错误 1753 年、十六进制错误 0x6d9 和友好错误"有可用的更多端点端点映射程序"。</para>
      <para>成员计算机的 IP 地址 x.x.1.2 更高版本，获取作为 contoso.com 域中的"MayberryDC"副本进行升级。再次<ui>立即复制</ui>命令用于触发复制，但这一次失败，屏幕错误"目标主要名称是错误。"计算机它的网络适配器分配的 IP 地址 x.x.1.2<placeholder>是</placeholder>域控制器，当前正常模式启动，并且已注册 E351...复制服务 UUID 使用其当地的企业项目管理，但它不拥有 DC2 名称或安全身份，并且无法解密 DC1 Kerberos 请求，以便请求现在失败，错误"目标主要名称不对。"该错误映射到小数点错误-2146893022 / 十六进制 0x80090322 错误。</para>
      <para>此类无效主机-TO-IP 映射可能导致过时中的项主机 / lmhost 文件主机 A / 中 DNS、WINS AAAA 登记。</para>
      <para>摘要：本例失败，因为无效主机 TO-IP 映射（在本例中的主文件）导致目标 DC 解决到"源"没有 Active Directory 域服务的 DC 服务运行（或甚至是安装的问题）因此 SPN 未尚未注册的复制和 DC 返回错误 1753 年的来源。在第二个的情况下，无效主机 TO-IP 映射（再次主机文件）中的导致目标 DC 连接到已注册 E351...DC 复制 SPN，但该源比预期源 DC 了不同的主机名称和安全身份，因此尝试失败，错误-2146893022：目标主要名称不正确。</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>故障排除失败，错误 1753 年的 Active Directory 操作：没有可用端点映射程序的更多端点。</linkText>
      <linkUri>https://support.microsoft.com/kb/2089874</linkUri>
    </externalLink>
<externalLink><linkText>知识库文章 839880 疑难解答 RPC 端点映射错误 Windows Server 2003 支持从使用工具产品 CD</linkText><linkUri>https://support.microsoft.com/kb/839880</linkUri></externalLink>
<externalLink><linkText>知识库文章 832017 服务概述和网络端口 Windows Server 系统要求</linkText><linkUri>https://support.microsoft.com/kb/832017/</linkUri></externalLink>
<externalLink><linkText>知识库文章 224196 限制 Active Directory 复制通信，客户端 RPC 通信到特定端口</linkText><linkUri>https://support.microsoft.com/kb/224196/</linkUri></externalLink>
<externalLink><linkText>知识库文章 154596 如何配置一起 RPC 端口动态分配使用防火墙</linkText><linkUri>https://support.microsoft.com/kb/154596</linkUri></externalLink><externalLink><linkText>RPC 的工作原理</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>如何服务器准备用于连接</linkText><linkUri>https://msdn.microsoft.com/library/aa373938(VS.85).aspx</linkUri></externalLink>
<externalLink><linkText>如何客户端建立连接</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>注册界面</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>推出服务器网络上</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>注册端点</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx</linkUri></externalLink><externalLink><linkText>侦听客户端呼叫</linkText><linkUri>https://msdn.microsoft.com/library/aa373966(VS.85).aspx</linkUri></externalLink><externalLink><linkText>如何客户端建立连接</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>限制 Active Directory 复制通信，客户端 RPC 通信到特定端口</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>目标 DC 广告 DS 中 SPN</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


