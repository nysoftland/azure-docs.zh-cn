---
title: 开始将警报从 OMS 扩展到 Azure | Microsoft Docs
description: 客户可以自愿执行用于将警报从 OMS 扩展到 Azure 的工具和 API。
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: 76b7481223566f16a5da8c08d9d76f2bdb6b542a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="initiate-extending-alerts-from-oms-into-azure"></a>开始将警报从 OMS 扩展到 Azure
从 **2018 年 4 月 23 日**开始，使用 [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) 中配置的警报的所有客户将扩展到 Azure。 扩展到 Azure 的警报的行为与在 OMS 中相同。 监视功能保持不变。 将 OMS 中创建的警报扩展到 Azure 可带来许多好处。 有关将警报从 OMS 扩展到 Azure 的优势和过程的详细信息，请参阅[将警报从 OMS 扩展到 Azure](monitoring-alerts-extend.md)。

想要立即将警报从 OMS 移到 Azure 的客户可以使用本文所述的选项之一。

## <a name="option-1---using-oms-portal"></a>选项 1 - 使用 OMS 门户
若要自愿开始将警报从 OMS 扩展到 Azure，请遵循下面所列的步骤。

1. 在 Operations Management Suite (OMS) 的“概述”页上转到“设置”，然后选择“警报”部分。 单击标有“扩展到 Azure”的按钮（参阅下图中突出显示的部分）。

    ![包含“扩展”选项的 OMS“警报设置”页](./media/monitor-alerts-extend/ExtendInto.png)

2. 单击该按钮后，将会显示一个向导，其中包括 3 个步骤，第一个步骤是提供过程详细信息。 请按“下一步”继续。

    ![将警报从 OMS 扩展到 Azure - 步骤 1](./media/monitor-alerts-extend/ExtendStep1.png)

3. 在第二个步骤中，系统会显示提议的更改摘要，其中列出了 OMS 中警报的相应[操作组](monitoring-action-groups.md)。 如果多个警报中出现了类似的操作，系统会提议将所有这些操作关联到单个操作组。  提议的操作组遵循命名约定 *WorkspaceName_AG_#Number*。 若要继续，请单击“下一步”。
下面显示了示例屏幕。

    ![将警报从 OMS 扩展到 Azure - 步骤 2](./media/monitor-alerts-extend/ExtendStep2.png)


4. 在向导的最后一个步骤，可以要求 OMS 计划将所有警报扩展到 Azure - 通过创建新操作组并将其关联到警报，如前面的屏幕中所示。 若要继续，请单击“完成”，并在提示中确认启动该过程。 （可选）客户还可以提供电子邮件地址，以便在完成处理后，OMS 向这些地址发送报告。

    ![将警报从 OMS 扩展到 Azure - 步骤 3](./media/monitor-alerts-extend/ExtendStep3.png)

5. 完成向导后，控件将返回到“警报设置”页，同时会删除“扩展到 Azure”选项。 OMS 会在后台计划将 OMS 中的警报扩展到 Azure；这可能需要一些时间。在操作执行一小段时间后，便不可以修改 OMS 中的警报。 将在标题中显示当前状态；如果在步骤 4 中提供了电子邮件地址，则在后台进程成功将所有警报扩展到 Azure 后，这些地址会收到通知。 

6. 即使已成功扩展到 Azure，警报也仍会列在 OMS 中。

    ![将 OMS 中的警报扩展到 Azure 之后](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>选项 2 - 使用 API
对于想要以编程方式控制或自动化完成将 OMS 中的警报扩展到 Azure 的客户，Microsoft 在 Log Analytics 下面提供了新的 AlertsVersion API。

Log Analytics 中的 AlertsVersion API 为 RESTful API，可通过 Azure 资源管理器 REST API 来访问。 在本文档中，可以找到使用 [ARMClient](https://github.com/projectkudu/ARMClient) 通过 PowerShell 命令行访问 API 的示例。ARMClient 是可简化 Azure 资源管理器 API 调用的开源命令行工具。 使用 ARMClient 和 PowerShell 是访问该 API 的多种方法之一。 API 将以 JSON 格式输出结果，从而允许以编程方式使用多种不同的方法使用结果。

针对 API 使用 GET，可以在结果中获取提议的更改摘要。此摘要信息以 JSON 格式显示 OMS 中警报的相应[操作组](monitoring-action-groups.md)列表。 如果多个警报中出现了类似的操作，系统会提议将所有这些操作关联到单个操作组。  提议的操作组遵循命名约定 *WorkspaceName_AG_#Number*。

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> 对 API 执行 GET 调用不会导致将 OMS 中的警报扩展到 Azure， 而只会以响应的形式提供提议的更改摘要。 若要确认是否执行了这些更改，以将警报扩展到 Azure，需要对 API 执行 POST 调用。

如果对 API 的 GET 调用成功并返回了 200 OK 响应，则会提供警报的 JSON 列表，以及提议的操作组。 示例响应如下：

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
如果指定的工作区中没有任何警报，则会连同 GET 操作的 200 OK 响应一起返回以下 JSON：

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

如果指定工作区中的所有警报已扩展到 Azure，则针对 GET 调用返回的响应为：
```json
{
    "version": 2
}
```

若要开始计划将 OMS 中的警报扩展到 Azure，请对 API 发起 POST 调用。 执行此调用/命令即确认用户有意且接受将他们在 OMS 中的警报扩展到 Azure，并根据针对 API 发出 GET 调用后返回的响应中的指示来执行更改。 （可选）用户可以提供一个电子邮件地址列表，以便在计划好的、将 OMS 中的警报扩展到 Azure 的后台进程成功完成后，OMS 向这些地址寄送报告。

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> 将 OMS 警报扩展到 Azure 的结果根据 GET 提供的摘要而有所不同 - 会考虑到在系统中执行的任何更改。 计划完成后，OMS 中的警报暂时不可编辑/修改 - 此时可以创建新警报。 

如果 POST 成功，应会返回 200 OK 响应和以下信息：
```json
{
    "version": 2
}
```
指示警报已扩展到 Azure，如 version 2 所示。 此版本仅用于检查警报是否已扩展到 Azure，在 [Log Analytics 搜索 API](../log-analytics/log-analytics-api-alerts.md) 中没有其他作用。 成功将警报扩展到 Azure 之后，与工作区中管理员和参与者角色关联的所有用户将收到一封电子邮件，其中包含所做的更改的详细信息。


最后，如果指定工作区中的所有警报已计划好要扩展到 Azure，则 POST 响应将是 403 Forbidden。


## <a name="next-steps"></a>后续步骤

* 详细了解新的 [Azure 警报体验](monitoring-overview-unified-alerts.md)。
* 了解 [Azure 警报中的日志警报](monitor-alerts-unified-log.md)。
