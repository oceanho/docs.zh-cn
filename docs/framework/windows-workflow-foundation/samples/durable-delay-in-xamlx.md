---
title: "XAMLX 中的持久延迟"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: efc38df4-2d34-453c-8e59-2c21d1307354
caps.latest.revision: "7"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.openlocfilehash: 7396c5ca2119dcf096036695233d5c89a3f014f7
ms.sourcegitcommit: ce279f2d7fe2220e6ea0a25a8a7a5370ddf8d9f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2017
---
# <a name="durable-delay-in-xamlx"></a>XAMLX 中的持久延迟
此示例演示如何使用持久延迟，在持久延迟过程中，将把工作流保留到持久性设备。  
  
> [!IMPORTANT]
>  您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  如果此目录不存在，请访问 [针对 .NET Framework 4 的 Windows Communication Foundation (WCF) 和 Windows Workflow Foundation (WF) 示例](http://go.microsoft.com/fwlink/?LinkId=150780) 以下载所有 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Services\DurableDelayXamlx`  
  
## <a name="discussion"></a>讨论  
 示例工作流包含发往一个本地文件并由延迟分隔的两个消息。 触发延迟时，工作流会卸载，并在重新加载到内存中之前，在工作流实例存储中等待 5 秒。  
  
 .xamlx 文件是在 [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)] 中承载的工作流服务。 [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)] 使用 Cassini，而 Cassini 使用工作流服务主机来承载工作流。  
  
 除了承载工作流之外，工作流服务主机还通过加载和卸载工作流实例来对其进行管理。 若要启动 [!INCLUDE[wf](../../../../includes/wf-md.md)] 定义的实例（在工作流服务主机上），请设置将向工作流中的 <xref:System.ServiceModel.Activities.Receive> 活动发送消息的客户端。 此 <xref:System.ServiceModel.Activities.Receive> 将自己的 <xref:System.ServiceModel.Activities.Receive.CanCreateInstance%2A> 属性设置为 `true`，从而可以在接收到消息时创建工作流的新实例。  
  
 初始化过程中，会将一个卸载实例行为添加到配置文件，该行为应在该配置文件指定到的工作流服务主机下，将实例卸载到持久性存储区（数据库）。 对于此示例，它在工作流进入空闲状态后（触发了延迟时）立即卸载实例。  
  
#### <a name="to-use-this-sample"></a>使用此示例  
  
1.  打开一个 [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] 命令提示符。  
  
2.  导航到 DurableDelayXamlx\CS 文件夹。  
  
3.  运行 Setup.cmd。  
  
4.  以管理员身份运行 [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)]。  
  
5.  打开 DurableDelayXamlx.sln 解决方案文件。  
  
6.  在**解决方案资源管理器**，右键单击该解决方案并选择**属性**。  
  
7.  选择**多启动项目**并将这两个项目设置为**启动**。  
  
8.  要生成解决方案，按 Ctrl+Shift+B。  
  
9. 若要运行解决方案，请按 Ctrl+F5。  
  
#### <a name="to-uninstall-this-sample"></a>卸载此示例  
  
1.  打开一个 [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] 命令提示符。  
  
2.  导航到 DurableDelayXamlx\CS 文件夹。  
  
3.  运行 Cleanup.cmd。  
  
> [!IMPORTANT]
>  您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  如果此目录不存在，请访问 [针对 .NET Framework 4 的 Windows Communication Foundation (WCF) 和 Windows Workflow Foundation (WF) 示例](http://go.microsoft.com/fwlink/?LinkId=150780) 以下载所有 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] 和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Services\DurableDelayXamlX`
