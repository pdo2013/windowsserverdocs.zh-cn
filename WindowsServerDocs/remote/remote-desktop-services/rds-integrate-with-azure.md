---
title: RDS-与 Azure 服务集成
description: 了解如何将 RDS 到你的 Azure 部署中与 Azure 集成到 RDS 部署。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.openlocfilehash: e582612496591356ed96b34522333d0e8bf34093
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879598"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>远程桌面服务-与 Azure 服务集成

Windows Server 2016 将组合桌面和应用程序通过使用 Microsoft Azure 提供的灵活、 可缩放服务的远程桌面服务的功能强大的安全交付。 你可以部署 RDS 与 Azure 服务来帮助降低基础结构的本地服务器的维护成本、 提高稳定性通过使用 Azure 服务以确保高可用性，通过使用多重身份验证提升安全性并提高你通过使用现有标识来访问 rds.中的资源的用户体验

使用以下信息将 Azure 整合到您的远程桌面部署：

- [了解如何对 RDS 使用多重身份验证](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [将 Azure AD 域服务与 RDS 部署集成](rds-azure-adds.md)
- [使用 Azure AD 应用程序代理发布远程桌面](/azure/active-directory/application-proxy-publish-remote-desktop)

若要了解这些服务简化的远程桌面部署的体系结构，请查看[RDS 体系结构和唯一的 Azure PaaS 角色](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles)。