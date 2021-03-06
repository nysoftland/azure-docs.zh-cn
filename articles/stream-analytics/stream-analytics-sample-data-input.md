---
title: Azure 流分析中的查询测试 | Microsoft 文档
description: 在对流分析作业进行故障排除时确定问题。
keywords: 对输入、输入采样进行故障排除
documentationcenter: ''
services: stream-analytics
author: jseb225
manager: ryanw
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: 99c9f7f2dc84f53d09b7aee1916c5532427de692
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2018
---
# <a name="azure-stream-analytics-query-testing-and-input-stream-sampling"></a>Azure 流分析查询测试和输入流采样

通过使用 Azure 流分析，可以对来自文件的输入事件进行采样，还可以测试门户中的查询，而无需启动或停止作业。

## <a name="testing-your-query"></a>测试查询

在流分析作业详细信息窗格中，单击“查询”下方的查询名称，打开“查询编辑器”边栏选项卡。 （在我们的示例方案中，由于尚未创建查询，因此请单击 < > 占位符。）

![流分析查询编辑器](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

用于创建查询的 RTF 编辑器边栏选项卡与先前版本中显示相同。 现在，已更新的边栏选项卡具有一个新的左窗格，它显示用于查询的、为此作业定义的输入和输出。

![流分析查询编辑器输入和输出列表](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

同时还显示了未定义的其他输入和输出。 它们来自你开始使用的新查询模板。 编辑查询时，它们会更改，甚至完全消失。 目前可以放心地忽略它们。

要使用示例输入数据进行测试，请右键单击任意输入，并选择“从文件上传示例数据”。

![文件命令中的流分析查询编辑器上传示例数据](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

完成上传后，单击“测试”，针对刚提供的示例数据来测试此查询。

![流分析查询编辑器测试按钮](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

要保存测试输出以供将来使用，浏览器中显示查询输出，以及下载结果链接。 现在可以轻松反复地修改查询，并对其进行重复测试，以查看输出有何变化。

![流分析查询编辑器示例输出](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

上图中添加了一个名为“HighAvgTempOutput”的第二个输出。

当在查询中使用多个输出时，可以单独查看每个输出的结果，并在它们之间轻松切换。

获得满意的结果后，可以保存查询、启动作业，享用流分析的神奇功能。

## <a name="get-help"></a>获取帮助

如需进一步的帮助，请试用我们的 [Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)。

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-real-time-fraud-detection.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)
