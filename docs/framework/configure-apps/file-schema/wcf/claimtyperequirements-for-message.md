---
title: "&lt;message&gt; 的 &lt;claimTypeRequirements&gt;"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: f95c5ecd-abb6-4b77-a6d7-a38727f4a142
caps.latest.revision: "4"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.openlocfilehash: 158aad6a6a11bb889df74e1222a8881a1c38803f
ms.sourcegitcommit: ce279f2d7fe2220e6ea0a25a8a7a5370ddf8d9f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2017
---
# <a name="ltclaimtyperequirementsgt-for-ltmessagegt"></a>&lt;message&gt; 的 &lt;claimTypeRequirements&gt;
指定所需声明类型的集合。  
  
 服务使用此集合来指定任何必选和可选声明，这些声明必须位于客户端用于访问服务的已颁发令牌中。 如果 WSDL 发布已启用但 WCF 并不要求已颁发的令牌包含指定的声明类型，则服务将会在元数据中公开所需的声明类型。 希望强制显示所需声明类型的服务应该使用授权策略。  
  
 在联合客户端上，此集合包含必选和可选声明的列表，该列表将在客户端请求已颁发令牌时发送给安全令牌服务。  
  
## <a name="see-also"></a>另请参阅  
 <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.ClaimTypeRequirements%2A>  
 <xref:System.ServiceModel.Security.Tokens.ClaimTypeRequirement>  
 <xref:System.ServiceModel.Configuration.FederatedMessageSecurityOverHttpElement.ClaimTypeRequirements%2A>  
 <xref:System.ServiceModel.Configuration.ClaimTypeElementCollection>  
 <xref:System.ServiceModel.Configuration.ClaimTypeElement>
