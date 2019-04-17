---
title: 开发工具扩展
description: 开发工具扩展 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 2269a2ac2cabda6f8fdd829994f36e89d581bd23
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296882"
---
# 在托管的节点上安装扩展负载

>适用于：Windows Admin Center、Windows Admin Center 预览版

## 安装
> [!NOTE]
> 若要遵循本指南，你将需要生成 1.2.1904.02001 或更高版本。 若要查看你的版本编号打开 Windows Admin Center，请单击右上角问号。

如果你尚未创建 Windows Admin center[工具扩展](../develop-tool.md)。 完成此生成后记创建扩展时使用的值：
| 值 | 说明 | 示例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 公司名称 （使用空格） | ```Contoso``` |
| ```{!Tool Name}``` | 你的工具名称 （使用空格） | ```InstallOnNode``` |

在工具扩展文件夹内创建```Node```文件夹 (```{!Tool Name}\Node```)。 任何内容放入此文件夹将复制到托管节点，使用此 API 时。 添加你的用例的任何文件。 

此外需要创建```{!Tool Name}\Node\installNode.ps1```脚本。 此脚本将托管的节点上运行，一旦所有文件都复制从```{!Tool Name}\Node```到托管节点的文件夹。 添加你的用例任何其他逻辑。 示例```{!Tool Name}\Node\installNode.ps1```文件：

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` 有一个特定的名称，该 API 将查找。 更改此文件的名称将导致错误。


## 与 UI 的集成

更新```\src\app\default.component.ts```为以下内容：

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
更新为创建扩展时所使用的值的占位符：
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

此外更新```\src\app\default.component.html```为：
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
和最后```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## 创建并安装 NuGet 程序包

最后一步是我们已添加的文件与生成 NuGet 程序包，然后在 Windows Admin Center 中安装该程序包。

如果尚未创建扩展包之前，请按照[发布扩展](../publish-extensions.md)指南操作。 
> [!IMPORTANT]
> 在此扩展.nuspec 文件中，它非常重要的```<id>```的值与你的项目中```manifest.json```和```<version>```匹配到增加了哪些功能```\src\app\default.component.ts```。 此外添加下的项```<files>```: 

> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

创建此程序包后，将添加到源中的路径。 Windows Admin Center 中转到设置 > 扩展 > 源并将其路径添加到存在该包。 完成你的扩展所安装，你应该能够单击```install```将会调用按钮和 API。  