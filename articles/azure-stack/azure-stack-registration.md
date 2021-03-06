---
title: Azure Stack 集成系统的 Azure 注册 | Microsoft Docs
description: 介绍多节点 Azure Stack Azure 连接部署的 Azure 注册过程。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: jeffgilb
ms.reviewer: avishwan
ms.openlocfilehash: 676dff1ae651d4754b96da52a68a9c7a7f35c2b8
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2018
---
# <a name="register-azure-stack-with-azure"></a>将 Azure Stack 注册到 Azure
注册[Azure 堆栈](azure-stack-poc.md)与 Azure 允许你从 Azure 下载应用商店项并设置回向 Microsoft 报告的商业数据。 注册 Azure 堆栈后，使用情况报告给 Azure 商务和你可以在用于注册的订阅下看到它。 

> [!IMPORTANT]
> 如果选择即用即付计费模式，则必须注册。 否则，你将会违反 Azure 堆栈部署的许可条款因为将不会报告使用情况。

## <a name="prerequisites"></a>必备组件
将 Azure Stack 注册到 Azure 之前，必须准备好：

- Azure 订阅的订阅 ID。 若要获取该 ID，请登录到 Azure，单击“更多服务” > “订阅”，单击要使用的订阅，然后，在“概要”下可以找到订阅 ID。 

  > [!NOTE]
  > 当前不支持德国和美国政府云订阅。

- 订阅所有者的帐户用户名和密码（支持 MSA/2FA 帐户）。
- 注册 Azure 堆栈资源提供程序 （请参阅下面有关详细信息的注册 Azure 堆栈资源提供程序部分）。

如果你没有满足这些要求的 Azure 订阅，则可以[创建免费的 Azure 帐户此处](https://azure.microsoft.com/free/?b=17.06)。 注册 Azure Stack 不会对 Azure 订阅收取任何费用。

### <a name="bkmk_powershell"></a>安装适用于 Azure Stack 的 PowerShell
你需要使用 Azure 堆栈的最新 PowerShell 将注册 Azure。

[安装适用于 Azure Stack 的 PowerShell](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install)（如果尚未安装）。 

### <a name="bkmk_tools"></a>下载 Azure Stack 工具
Azure Stack 工具 GitHub 存储库包含支持 Azure Stack 功能（包括注册功能）的 PowerShell 模块。 在注册过程中，你需要导入和使用 RegisterWithAzure.psm1 PowerShell 模块中，找到 Azure 堆栈工具储存库中，向 Azure 注册你的 Azure 堆栈实例。 

若要确保使用最新版本，你应删除任何现有版本的 Azure 堆栈工具和[从 GitHub 下载最新版本](azure-stack-powershell-download.md)之前注册 Azure。

## <a name="register-azure-stack-in-connected-environments"></a>在连接的环境中注册 Azure Stack
连接的环境可以访问 Internet 和 Azure。 对于这些环境，需将 Azure Stack 资源提供程序注册到 Azure，然后配置计费模式。

> [!NOTE]
> 所有这些步骤必须在可以访问特权终结点的计算机上运行。 

### <a name="register-the-azure-stack-resource-provider"></a>注册 Azure Stack 资源提供程序
若要向 Azure 注册 Azure 堆栈资源提供程序，请以管理员身份启动 PowerShell ISE，并使用以下 PowerShell 命令使用**EnvironmentName**参数设置为相应的 Azure 订阅类型 （请参阅以下参数）。 

1. 添加用于注册 Azure Stack 的 Azure 帐户。 若要添加的帐户，运行**添加 AzureRmAccount** cmdlet。 系统将提示你输入你的 Azure 全局管理员帐户凭据，并且你可能必须为使用双因素身份验证，具体情况视你的帐户的配置。

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | 参数 | 说明 |  
   |-----|-----|
   | EnvironmentName | Azure 云订阅环境名称。 受支持的环境名称**AzureCloud**或者，如果使用中国 Azure 订阅， **AzureChinaCloud**。  |
   |  |  |

2. 如果有多个订阅，请运行以下命令，选择要使用的那个订阅：  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. 运行以下命令，在 Azure 订阅中注册 Azure Stack 资源提供程序：

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>使用即用即付计费模式将 Azure Stack 注册到 Azure
使用以下步骤 Azure 堆栈向 Azure 注册以使用为你的使用付费计费模型。

1. 以管理员身份启动 PowerShell ISE，并导航到[下载 Azure Stack 工具](#bkmk_tools)时所创建的 **AzureStack-Tools-master** 目录中的 **Registration** 文件夹。 使用 PowerShell 导入 **RegisterWithAzure.psm1** 模块： 

  ```powershell
  Import-Module .\RegisterWithAzure.psm1
  ```

2. 接下来，在相同的 PowerShell 会话中，确保你已登录到正确的 Azure PowerShell 上下文。 这是用于注册上述 Azure 堆栈资源提供程序的 azure 帐户。 若要运行的 Powershell: 

  ```powershell 
  Login-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>" 
  ``` 

3. 在相同的 PowerShell 会话中，运行**集 AzsRegistration** cmdlet。 要运行的 PowerShell：  

  ```powershell
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse
  ```

  |参数|说明|
  |-----|-----|
  |PrivilegedEndpointCredential|使用到的凭据[访问特权终结点](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)。 用户名采用以下格式**AzureStackDomain\CloudAdmin**。|
  |PrivilegedEndpoint|预先配置的远程 PowerShell 控制台，提供的功能包括日志收集和其他部署后任务。 有关详细信息，请参阅[使用特权终结点](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint)一文。|
  |BillingModel|订阅使用的计费模式。 此参数允许的值：Capacity、PayAsYouUse 和 Development。|
  |  |  |

  过程需要介于 10 到 15 分钟之间。 命令完成后，你将看到消息**"你的环境现在已注册和激活使用提供的参数。"**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>使用容量计费模式将 Azure Stack 注册到 Azure
按照相同的说明用于注册使用作为你的使用付费计费模型，但添加在其下购买容量的协议编号，并更改**BillingModel**参数**容量**. 其他所有参数不变。

要运行的 PowerShell：

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>在离线环境中注册 Azure Stack 
*本部分中的信息适用于 Azure Stack 1712 更新版 (180106.1) 和更高版本，不支持更低的版本。*

若要在离线环境（未建立 Internet 连接）中注册 Azure Stack，需要从 Azure Stack 环境获取注册令牌，然后在可连接到 Azure 并已[安装适用于 Azure Stack 的 PowerShell](#bkmk_powershell) 的计算机上使用该令牌。  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>从 Azure Stack 环境获取注册令牌

1. 以管理员身份启动 PowerShell ISE，并导航到[下载 Azure Stack 工具](#bkmk_tools)时所创建的 **AzureStack-Tools-master** 目录中的 **Registration** 文件夹。 导入 **RegisterWithAzure.psm1** 模块：  

  ```powershell 
  Import-Module .\RegisterWithAzure.psm1 
  ``` 

2. 若要获取注册令牌，请运行以下 PowerShell 命令：  

  ```Powershell
  $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
  $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
  ```
  
  > [!TIP]  
  > 注册令牌保存在为指定的文件*$FilePathForRegistrationToken*。 你可以自行更改文件路径或文件名。 

3. 保存此注册令牌，以便在连接 Azure 的计算机上使用。 你可以从 $FilePathForRegistrationToken 复制文件或文本。


### <a name="connect-to-azure-and-register"></a>连接到 Azure 并注册
计算机上 internet 连接，执行相同的步骤向正确的 Azure Powershell 上下文中导入的 RegisterWithAzure.psm1 模块和登录名。 然后，调用注册 AzsEnvironment 并指定要向 Azure 注册的注册令牌：

  ```Powershell  
  $registrationToken = "<Your Registration Token>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```
（可选）可以使用 Get-Content cmdlet，使之指向包含注册令牌的文件：

  ```Powershell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```
  > [!NOTE]  
  > 保存注册资源名称和注册令牌以供将来参考。

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>从 Azure 注册资源检索激活密钥 
接下来，你需要从在 Azure 中创建期间注册 AzsEnvironment 注册资源检索激活密钥。 
 
若要获取激活密钥，请运行以下 PowerShell 命令：  

  ```Powershell 
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>" 
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt" 
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath 
  ``` 
  > [!TIP]   
  > 激活密钥保存在为指定的文件*$KeyOutputFilePath*。 你可以自行更改文件路径或文件名。 

### <a name="create-an-activation-resource-in-azure-stack"></a>在 Azure 堆栈中创建激活资源 
从返回到 Azure 堆栈环境使用的文件或文本创建从 Get AzsActivationKey 的激活密钥。 接下来你将使用的激活密钥的 Azure 堆栈中创建激活资源。 若要创建激活资源，请运行以下 PowerShell 命令：  

  ```Powershell 
  $ActivationKey = "<activation key>" 
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey 
  ``` 

（可选）可以使用 Get-Content cmdlet，使之指向包含注册令牌的文件： 

  ```Powershell   
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>' 
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey 
  ``` 

## <a name="verify-azure-stack-registration"></a>验证 Azure Stack 注册
使用以下步骤来验证 Azure Stack 是否已成功注册到 Azure。
1. 登录到 Azure Stack [管理员门户](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal)：https&#58;//adminportal.*&lt;区域>.&lt;fqdn>*。
2. 单击“更多服务” > “Marketplace 管理” > “从 Azure 添加”。

如果看到 Azure 提供的项列表（例如 WordPress），则表示激活成功。 但是，在连接断开的环境中你将不看到 Azure 堆栈应用商店中的 Azure 应用商店项。

> [!NOTE]
> 完成注册后，将不再显示提示未注册的活动警告。

## <a name="renew-or-change-registration"></a>续订或更改注册
### <a name="renew-or-change-registration-in-connected-environments"></a>续订或更改连接的环境中的注册
在以下情况下，需要更新或续订注册：
- 续订基于容量的年度订阅之后。
- 更改计费模式时。
- 调整基于容量的计费（添加/删除节点）时。

#### <a name="change-the-subscription-you-use"></a>更改使用的订阅
若要更改使用的订阅，必须先运行 **Remove-AzsRegistration** cmdlet，然后确保登录到正确的 Azure PowerShell 上下文，最后使用任何已更改的参数来运行 **Set-AzsRegistration**：

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>更改计费模式或联合功能
若要更改安装的计费模式或联合功能，可以调用注册函数来设置新值。 不需要先删除当前注册： 

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>续订或更改在连接断开的环境中的注册 
在以下情况下，需要更新或续订注册： 
- 续订基于容量的年度订阅之后。 
- 更改计费模式时。 
- 调整基于容量的计费（添加/删除节点）时。 

#### <a name="remove-the-activation-resource-from-azure-stack"></a>从 Azure 堆栈中删除激活资源 
你首先需要从 Azure 堆栈，然后在 Azure 中的注册资源中删除激活资源。  

若要在 Azure 堆栈中删除激活资源，请在你的 Azure 堆栈环境中运行以下 PowerShell 命令：  

  ```Powershell 
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint 
  ``` 

接下来，若要在 Azure 中删除注册资源，请确保你上连接 Azure 计算机，登录到正确的 Azure PowerShell 上下文中，并运行相应的 PowerShell 命令，如下所述。

你可以使用用于创建资源的注册令牌：  

  ```Powershell 
  $registrationToken = "<registration token>" 
  Unregister-AzsEnvironment -RegistrationToken $registrationToken 
  ``` 
  
或者，你可以使用的注册名称： 

  ```Powershell 
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>" 
  Unregister-AzsEnvironment -RegistrationName $registrationName 
  ``` 

### <a name="re-register-using-disconnected-steps"></a>使用断开连接的步骤重新注册 
你现在完全取消注册在断开连接的情况下，必须在断开连接的情况下注册 Azure 堆栈环境重复步骤。 

## <a name="next-steps"></a>后续步骤

[从 Azure 下载应用商店项](azure-stack-download-azure-marketplace-item.md)