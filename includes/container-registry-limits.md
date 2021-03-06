---
title: include 文件
description: include 文件
services: container-registry
author: mmacy
ms.service: container-registry
ms.topic: include
ms.date: 03/23/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 575483192954f4e05db50e701e223829e041cffc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
| 资源 | 基本 | 标准 | 高级 |
|---|---|---|---|---|
| 存储 | 10 GiB | 100 GiB| 500 GiB |
| 每分钟读取操作数<sup>1、2</sup> | 1000 | 3000 | 10000 |
| 每分钟写入操作数<sup>1、3</sup> | 100 | 500 | 2000 |
| 下载带宽 (MBps)<sup>1</sup> | 30 | 60 | 100 |
| 上传带宽 (MBps)<sup>1</sup> | 10 | 20 | 50 |
| Webhook | 2 | 10 | 100 |
| 异地复制 | 不适用 | 不适用 | [支持（预览版）](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup>读取操作数、写入操作数和带宽是最小估计值。 ACR 旨在随使用情况增多提升性能。

<sup>2</sup>[docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) 根据映像中的层数和清单检索行为转换为多个读取操作。

<sup>3</sup>[docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) 根据必须推送的层数转换为多个写入操作。 `docker push` 包含 ReadOps，用于检索现有映像的清单。