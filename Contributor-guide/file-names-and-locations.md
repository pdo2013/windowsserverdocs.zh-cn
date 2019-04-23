<properties title="" pageTitle="文件的名称和位置的 Windows Server 2016 技术文章" description="介绍项目情况以及创建新文章时应遵循的命名约定的文件的结构。" metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy-Davies" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="jimpark; tysonn" />

# <a name="file-names-and-locations-for-windows-server-technical-articles"></a>文件的名称和位置的 Windows Server 技术文章

了解和遵循的规则可帮助您获取更快地接受你的拉取请求。

+ [规则]
+ [Pattern]
+ [文件名审批]

## <a name="rules"></a>规则

- 所有文件必须是在 markdown 中并且使用.md 文件扩展名。
- 有可能为 3 到 5 的字词将文件名称-10 个真的是太长时间以提高可读性并 SEO。
- 使用大小写和字母、 数字和连字符。
- 无空格或标点字符。 使用连字符分隔单词和数字中的文件的名称。
- 不使用操作谓词的缩写形式-ing 窗体：`deploy-nano-server`不 `Deploying-Nano-Server`
- 在中，保留出小词，例如 a、 and、、 或。
- 拼写单词避免在文件名中的未批准或不必要首字母缩写
- 文件的名称应该是唯一的而不是`overview.md`使用 `storage-spaces-overview.md`

首字母缩写词，并在文件名称的特定准则中的缩略词：

- 请按照可接受名缩写的现有 Microsoft 指南
- 行业标准缩写词是根据需要在文件名中可接受的。

## <a name="pattern"></a>模式

查看要了解的现有名称的存储库中的项目的列表。 以下是常规模式：

 **component-topic-title.md**
 
例如，`storage-spaces-direct-overview.md`

## <a name="file-name-approval"></a>文件名审批

当您提交的新文件时，拉取请求审阅者检查名称，如果需要更改通过拉取请求注释流提供反馈。 文件名称必须进行更正，然后接受拉取请求。 参与者可以轻松地将更新推送到挂起的拉取请求。

## <a name="folders-in-the-repo"></a>存储库中的文件夹

使用现有的文件夹结构。 不创建文件夹，而无需获得批准存储库管理员。如果您认为您需要一个新文件夹与它们进行通信。

GitHub 存储库应具有一个简单和相对扁平的文件夹结构 –\<区域 >\\\<技术 >-示例 storage\data 的重复数据删除。 执行此操作具有以下优点：
 - 这其实非常简单。
 - 它是更接近于 Azure.com 的工作原理
 - 它轻松编写器/Pm 快速查找其主题 – 无需研究寻找其中某项技术嵌套的其他文件夹。
 - 它会短，这是适合 SEO 的 URL，用户体验，客户偶尔查看提示的 URL。

## <a name="changing-case-in-file-names"></a>更改文件名称中的大小写

Windows 操作系统是不区分大小写。 如果需要更改文件名称，若要修复的大小写，则最好更改文件内容，除非您是在 Linux 或 mac。 例如：

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services --> biztalk-services-administration-and-development-task-list

使用`git mv`命令重命名文件：
```
  git mv <WindowsServerDocs/tech-area/subarea/current-file-name.md> <WindowsServerDocs/tech-area/subarea/new-file-name.md>
```

### <a name="contributors-guide-links"></a>参与者的指南的链接

- [指南文章的索引](./contributor-guide-index.md)


<!--Anchors-->
[规则]: #rules
[Pattern]: #pattern
[文件名审批]: #file-name-approval
