---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: "配置计算机问题的疑难解答"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 915b8d1133b3bee050f5eedce9e7ac445f833048
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-a-computer-for-troubleshooting"></a>配置计算机问题的疑难解答

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>使用高级故障排除方法来识别并修复 Active Directory 问题之前，将配置你的计算机问题的疑难解答。 你还应该的基本了解<token>nextref_longhorincludes > 疑难解答概念、 过程和工具。 </para>
    <para>For information about monitoring tools for Windows Server 2008, see the Step-by-Step Guide for Performance and Reliability Monitoring in Windows Server 2008 (<externalLink><linkText>https://go.microsoft.com/fwlink/?LinkId=123737</linkText><linkUri>https://go.microsoft.com/fwlink/?LinkId=123737</linkUri></externalLink>).</para>
  </introduction>
  <section>
    <title>以获取疑难解答配置任务</title>
    <content>
      <para>要配置你的计算机问题的疑难解答 Active Directory 域服务 (广告 DS)，请执行以下任务：</para>
      <para>
        <link xlink:href="#BKMK_2">安装广告 DS 的远程服务器管理工具</link>
      </para>
      <para>
        <link xlink:href="#BKMK_3">配置可靠性和性能监视器</link>
      </para>
      <para>
        <link xlink:href="#BKMK_4">设置日志级别</link>
      </para>
    </content>
    <sections>
      <section address="BKMK_2">
        <title>安装广告 DS 远程服务器管理工具</title>
        <content>
          <para>管理工具，用于管理广告 DS 广告 DS 创建域控制器安装时，将自动安装。 如果你想要从的计算机不是域控制器远程管理域控制器，你可以安装上运行的成员服务器管理工具<token>nextref_longhorincludes > 或运行的 Windows Vista 的 Service Pack 1 (SP1) 的计算机上。 在正在运行的成员服务器<token>nextref_longhorincludes >，你使用远程服务器管理工具 (RSAT) 功能服务器管理器中安装 Active Directory 域名服务工具。 RSAT 替换 Windows 在 Windows Server 2003 的支持工具。 你可以通过到这台计算机下载工具运行的 Windows Vista 的 Service Pack 1 (SP1) 的计算机上安装 Active Directory 域名服务工具。</para>
          <para>有关安装 RSAT 的信息，请参阅<link xlink:href="610ba7d9-51b5-4e14-9232-0510a9091aba">安装广告 DS 的远程服务器管理工具</link>。</para>
        </content>
      </section>
      <section address="BKMK_3">
        <title>配置可靠性和性能监视器</title>
        <content>
          <para>Windows Server 2008 包含 Windows 的可靠性和性能监视器是 Microsoft 管理控制台 (MMC) 管理单元，它结合以前独立的工具，包括性能的日志和通知、 Server 性能顾问，并系统监视器的功能。管理单元收集的数据集和事件跟踪会话自定义提供图形用户界面 (GUI)。 </para>
          <para>可靠性和性能监视器还包括可靠性监视器，贴靠 mmc 系统跟踪更改，并将它们进行比较更改在系统稳定性，提供它们的关系的图形视图。 </para>
        </content>
      </section>
      <section address="BKMK_4">
        <title>设置日志级别</title>
        <content>
          <para>如果你收到目录服务日志事件查看器中的信息不足以获取疑难解答中，通过使用中的相应的注册表项来提升日志记录级别 <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>。 </para>
          <para>所有条目的日志记录级别设置为默认情况下 <embeddedLabel>0</embeddedLabel>，提供最少信息。最高的日志记录级别 <embeddedLabel>5</embeddedLabel>。增加条目级别使其他事件中目录服务事件日志记录。 </para>
          <para>可以使用下面的过程中更改日志记录级别的诊断项。 </para>
          <alert class="caution">
            <para>我们建议你执行不直接编辑注册表除非没有没有其他选择。对注册表进行修改不验证注册表编辑器中或通过 Windows 应用，并可以存储错误值的结果之前。这可以无法恢复错误导致系统中。如果可能，请使用组策略或其他 Windows 工具，例如管理单元，来完成任务，而不是编辑注册表直接。如果你必须编辑注册表，使用格外小心。 </para>
          </alert>
          <para>
            <embeddedLabel>要求</embeddedLabel>
          </para>
          <list class="bullet">
            <listItem>
              <para>中的成员 <embeddedLabel>域管理员</embeddedLabel>，或等效的最低要求完成此过程。 <token>review_detailincludes></para>
            </listItem>
            <listItem>
              <para>工具： Regedit.exe</para>
            </listItem>
          </list>
          <procedure>
            <title>更改日志记录级别的诊断项</title>
            <steps class="ordered">
              <step>
                <content>
                  <para>单击<ui>开始</ui>，单击<ui>运行</ui>，类型 <userInput>regedit</userInput>，然后单击<ui>确定</ui>。</para>
                </content>
              </step>
              <step>
                <content>
                  <para>导航到所需设置登录的条目 <embeddedLabel>HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics</embeddedLabel>。</para>
                </content>
              </step>
              <step>
                <content>
                  <para>双击条目，并在 <embeddedLabel>基</embeddedLabel>，单击<embeddedLabel>小数点</embeddedLabel>。</para>
                </content>
              </step>
              <step>
                <content>
                  <para>中<embeddedLabel>值</embeddedLabel>，键入一个介于 <embeddedLabel>0</embeddedLabel> 通过 <embeddedLabel>5</embeddedLabel>，然后单击<ui>确定</ui>。</para>
                </content>
              </step>
            </steps>
          </procedure>
        </content>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>


