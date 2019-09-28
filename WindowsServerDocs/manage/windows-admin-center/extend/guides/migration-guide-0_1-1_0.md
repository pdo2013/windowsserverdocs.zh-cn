---
title: 从 Windows 管理中心 SDK 0.1 迁移到1。0
description: 本指南将帮助你从 Windows 管理中心 SDK 0.1 版迁移到1。0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c52870178e7caff0abc8ddcccc62966d637dd3c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357056"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>从 Windows 管理中心 SDK 0.1 迁移到1。0

>适用于：Windows Admin Center 预览版

本指南将帮助你从 Windows 管理中心 SDK 版本0.1 升级到1.0。  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1.了解开发人员指南扩展的新控件

Windows 管理中心版本1902及更高版本包含**开发人员指南**扩展，可用于查找控件示例（包括新提供的控件）和方案，以帮助你生成自己的扩展。  开发指南取代了 SDK 早期版本中的**开发人员工具**扩展。

### <a name="use-the-dev-guide-in-windows-admin-center"></a>使用 Windows 管理中心中的开发指南

开发人员指南在 Windows 管理中心版本1902及更高版本中以解决方案形式提供。  开发人员指南已预安装，但需要通过设置启用。

**在 Windows 管理中心中启用开发指南：**

* 打开 Windows 管理中心（版本1902及更高版本）
* 单击窗口右上角的 "**设置**" 图标
* 选择 "**高级**" 选项卡
* 在 "*试验密钥*" 下，单击 "**添加**"
* 在上一步创建的空字段中输入新值 ```msft.sme.shell.devguide```
* 单击 "**保存并重新加载**"

**在 Windows 管理中心中打开开发指南：**

* 打开 Windows 管理中心（版本1902及更高版本）
* 单击左上角的下拉框以显示所有解决方案类型
* 选择**开发指南**解决方案 
    * 如果看不到列出的解决方案，请确保已启用开发指南（请参阅上面的部分）并重新加载 Windows 管理中心。
* 通过选择其中一个选项卡浏览开发指南的内容
    * **登录**包含*管理 As*和*通知*方案的代码示例
    * **控件**包含 SDK 中每个可用控件的示例
    * **布线**包含可用转换器和格式化程序函数的示例
    * **字体**包含 SDK 中可用的 CSS 样式的示例
    * **MsftSme:** 包含高级方案的示例和指南 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>浏览 GitHub 上开发人员指南的源代码

可以浏览 GitHub 上的开发指南[源代码](https://github.com/Microsoft/windows-admin-center-sdk/)，查找示例 HTML、CSS 和 TypeScript 代码示例。

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2.准备适用于最新 SDK 的开发环境

安装或更新 node.js 版本[10.15.1 LTS 或更高](https://nodejs.org/en/)版本。

将 Windows 管理中心 CLI 更新到最新版本：

[//]: # "npm 卸载-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

将全局依赖项更新为以下版本：

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3.使用最新的 SDK 创建新项目

使用 Windows 管理中心 CLI 创建面向 ```next``` 版本（SDK 1.0）的新项目：

[//]: # "wac 创建--公司 "Contoso Inc."--工具 "管理 Foo 工作"--实验版本"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

接下来，将目录更改为刚创建的文件夹，然后通过运行 @no__t 安装所需的本地依赖项。

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4.修改现有项目以使用最新的 SDK

重要：在继续操作之前，对项目进行备份。

修改 ```package.json``` 中的以下行以面向 @no__t 版本（SDK 1.0）：

[//]: # ""@no__t"： "试验""

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

然后，运行 ```npm install``` 以便更新整个项目中的引用。

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5.使用 SDK CLI 修复常见的迁移问题

重要：在继续操作之前，对项目进行备份。

从项目的根文件夹中，对项目运行以下 CLI 命令，以自动修复常见的迁移问题：

``` cmd
wac updateSeven --update
```

此 CLI 命令自动解决以下问题：

* 重新生成 ```package-lock.json```
* 在角度编译环境中更新文件：
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6.使用 SDK CLI 了解常见的迁移问题

从项目的根文件夹中，运行以下 CLI 命令审核项目，查找需要手动解决的常见迁移问题：

``` cmd
wac updateSeven --audit
```

这会在项目中找到以下问题的实例：

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>将以下 CSS 类的用法替换为这些 sme 类：

| 旧 CSS 类 | 新建 CSS 类 |
| -- | -- |
| 。自动调整弹性大小 |  sme-位置-弹性-自动 |
| . border-全部 |  . sme-带边框的-sm 和90边框 |
| 。下边框 |  "sme"-"下边框-sm" 90 和 "" |
| 。边框-水平 |  "sme-水平-sm" 和 "sme-水平-水平-90" |
| 。边框-左对齐 |  "sme-左-sm" 和 "sme-左边框-90" |
| 。边框-右 |  "sme-右-sm" 和 "sme-右边框-90" |
| 。上边框-顶部 |  "sme-顶部-sm" 和 "sme-顶部-最高颜色-90" |
| 。边框-垂直 |  "sme-垂直-sm" 和 "sme-垂直-垂直-90" |
| 。 break-word |  . sme-双包装 |
| 。 btn |  sme-按钮或按钮 |
| 。 btn-主要 |  . sme-button-primary 或 ... 按钮-主要 |
| 。颜色-深色 |  sme-彩色-alt |
| 。颜色-浅色 |  sme-颜色基数 |
| 。颜色浅-灰色 |  sme-彩色-90 |
| 。固定弹性大小 |  sme-位置-弹性-无 |
| 。弹性-布局 |  -a--stack-h 或。 |
| 。字体-粗体 |  . emphasis1 |
| 。突出显示 |  sme-背景色-黄色 |
| 。水平 |  --排布-h |
| 。非滚动 |  sme-位置-弹性-自动 |
| 。 nowrap |  -a--stack-h 或。 |
| 。相对 |  sme-布局-相对 |
| 。相对中心 |  sme-布局-绝对的 |
| 。反向 |  . sme-堆栈反向 |
| 。 stretch-绝对 |  sme-布局-绝对内容-位置-内陷-无 |
| 。 stretch-已修复 |  sme-已修复-位置-内陷-无 |
| 。拉伸-垂直 |  sme-位置-stretch |
| . stretch-宽度 |  sme-位置-stretch-h |
| vertical |  . sme-stack |
| 。垂直-仅滚动 |  . sme –溢出-hide-x sme –溢出-auto y |
| wrap |  wrapstack-h 或-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>将以下组件的使用替换为这些 sme 组件：

| 旧组件 | 新建组件 |
| -- | -- |
| 。警报 |  sme-警报 |
| 。警报-危险 |  sme-警报 |
| 痕迹 |  sme-警报 |
| 。复选框 |  sme 窗体-字段 [type = "checkbox"] |
| combobox |  sme 窗体-字段 [type = "select"] |
| 。仪表板 |  sme-布局-内容-区域填充的 sme-排列-h |
| 。详细信息-面板 |  sme-属性网格 |
| 。详细信息-面板-容器 |  sme-属性网格 |
| 。详细信息-选项卡 |  sme-属性网格或 sme-透视 |
| 。详细信息-包装器 |  sme-属性网格 |
| 。已禁用 |  sme-已禁用 |
| 。窗体-按钮 | sme 窗体-字段 |
| 。窗体控件 | sme 窗体-字段 |
| 。窗体控件 | sme 窗体-字段 |
| 。窗体-组 | sme 窗体-字段 |
| 。窗体组标签 | sme 窗体-字段 |
| 。窗体-输入 | sme 窗体-字段 |
| 。窗体-stretch | sme 窗体-字段 |
| . 输入-文件 | sme 窗体-字段 |
| 。导航选项卡 |  sme-透视 |
| 。单选 |  sme 窗体-字段 [type = "单选"] |
| 。必需-提示 | sme 窗体-字段 |
| 。 searchbox |  sme 窗体-字段 [type = "search"] |
| 。切换-切换 |  sme 窗体-字段 [type = "切换-切换"] |
| 。工具-容器 |  sme-布局-内容区域或 sme-布局-内容-区域填充 |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>这些 CSS 类已弃用，不再受支持：

| 旧类 | 已弃用 |
| -- | -- |
| 。可接受 | 弃用 |
| 。颜色-错误 | 弃用 |
| 。颜色-信息 | 弃用 |
| 。颜色-成功 | 弃用 |
| 。颜色-警告 | 弃用 |
| 。删除-按钮 | 弃用 |
| 。详细信息-内容 | 弃用 |
| 。错误-覆盖 | 弃用 |
| 。错误-消息 | 弃用 |
| 。引导式窗格按钮 | 弃用 |
| 。标头-容器 | 弃用 |
| 。图标-win | 弃用 |
| 。缩进 | 弃用 |
| 。无效 | 弃用 |
| . 项-列表 | 弃用 |
| 。模式-可滚动 | 弃用 |
| 。多节 | 弃用 |
| 。无操作栏 | 弃用 |
| 。溢出-边距 | 弃用 |
| 。溢出工具 | 弃用 |
| 。进度-覆盖 | 弃用 |
| 。右面板 | 弃用 |
| 。汇总 | 弃用 |
| 。汇总-状态 | 弃用 |
| 。汇总-标题 | 弃用 |
| 。汇总-值 | 弃用 |
| 。 searchbox-操作栏 | 弃用 |
| 。大小-h-1 | 弃用 |
| 。大小-h-2 | 弃用 |
| 。大小-h-3 | 弃用 |
| 。大小-h-4 | 弃用 |
| 。大小-h-完全 | 弃用 |
| 。大小-h-半 | 弃用 |
| 。大小-v-1 | 弃用 |
| 。大小-v-2 | 弃用 |
| 。大小-v-3 | 弃用 |
| 。大小-v-4 | 弃用 |
| 。状态-图标 | 弃用 |
| 。 svg-16px | 弃用 |
| 。表缩进 | 弃用 |
| 。表-sm | 弃用 |
| 。瘦 | 弃用 |
| . 磁贴 | 弃用 |
| . 磁贴-正文 | 弃用 |
| . 磁贴-内容 | 弃用 |
| . 磁贴页脚 | 弃用 |
| . 磁贴-标头 | 弃用 |
| . 磁贴-布局 | 弃用 |
| . 磁贴-表 | 弃用 |
| 。工具栏 | 弃用 |
| 。工具条 | 弃用 |
| . 工具标头 | 弃用 |
| . 工具标头-框 | 弃用 |
| 。工具窗格 | 弃用 |
| 。用法-bar | 弃用 |
| 。用法-条形图-面积图 | 弃用 |
| 。用法-bar-背景 | 弃用 |
| 。用法-条形图-标题 | 弃用 |
| 。用法-条形值 | 弃用 |
| 。用法-图表 | 弃用 |
| 。用法-消息 | 弃用 |
| 。使用情况-消息区域 | 弃用 |
| 。用法-消息标题 | 弃用 |
| 。警告 | 弃用 |
| 。空白 | 弃用 |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7.了解和解决可观察对象的问题

### <a name="update--rxjs-function-use-for-observable-objects"></a>更新 ```rxjs``` 函数用于可观察对象

这些是已更改的一些常见函数名称，你的项目中可能还有其他一些名称。

* 更新 ```Observable.empty()``` 到 ```empty()```
* 更新 ```Observable.of()``` 到 ```of()```
* 更新 ```.switchMap()``` 到 ```.pipe(switchMap())```
* 更新 ```.map()``` 到 ```.pipe(map())```
* 更新 ```flatMap()``` 到 ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>解决可观察对象上 @no__t 0 和 @no__t 1 函数的运行时问题

如果编译器无法正确确定 @no__t 0 对象的类型，则可以改为将 ```array``` 对象中的 ```.map()``` 和 ```.filter()``` 函数映射到你的对象，从而导致运行时错误。  请确保函数返回指定了显式数据类型的 @no__t 0 对象，以避免此问题。

@no__t 0 和不返回类型都可能导致此问题，请查找具有以下模式的代码：

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8.解决其他常见问题

这些方法可帮助解决其他常见问题：

* 运行 ```ng lint --fix``` 以修复常见不起毛的问题
* 重复运行 ```gulp build``` 以增量修复 ```gulp build``` 可以自动解决的问题

## <a name="9-build-and-serve-your-project"></a>9.构建项目并为其提供服务

运行以下命令以生成项目并为其提供最新版本（SDK 1.0）：

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10.在 Windows 管理中心中打开深色主题

若要在 Windows 管理中心版本1902及更高版本中打开深色主题，请执行以下步骤：

* 打开 Windows 管理中心（版本1902及更高版本）
* 单击窗口右上角的 "**设置**" 图标
* 选择 "**高级**" 选项卡
* 在 "*试验密钥*" 下，单击 "**添加**"
* 在上一步创建的空字段中输入新值 ```msft.sme.shell.personalization```
* 单击 "**保存并重新加载**"
* 设置现在具有新的选项卡 "**个性化**"。  选择此选项卡
* 将**颜色**更改为**深色模式（预览）**
