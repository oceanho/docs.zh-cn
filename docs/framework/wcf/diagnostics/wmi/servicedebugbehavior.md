---
title: ServiceDebugBehavior
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: a5ec9061-1e95-43fb-b0d9-dbd0a7bc3c44
caps.latest.revision: "7"
author: dotnet-bot
ms.author: dotnetcontent
manager: wpickett
ms.openlocfilehash: 5c829f68fe994b47fe880febb4d3ac2020fde2d1
ms.sourcegitcommit: ce279f2d7fe2220e6ea0a25a8a7a5370ddf8d9f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2017
---
# <a name="servicedebugbehavior"></a>ServiceDebugBehavior
ServiceDebugBehavior  
  
## <a name="syntax"></a>语法  
  
```  
class ServiceDebugBehavior : Behavior  
{  
  boolean HttpHelpPageEnabled;  
  string HttpHelpPageUrl;  
  boolean HttpsHelpPageEnabled;  
  string HttpsHelpPageUrl;  
  boolean IncludeExceptionDetailInFaults;  
};  
```  
  
## <a name="methods"></a>方法  
 ServiceDebugBehavior 类未定义任何方法。  
  
## <a name="properties"></a>属性  
 ServiceDebugBehavior 类具有以下属性：  
  
### <a name="httphelppageenabled"></a>HttpHelpPageEnabled  
 数据类型：Boolean  
  
 访问类型：只读  
  
 控制服务是否在 `HttpGetUrl` 属性控制的地址上发布其 WSDL。  
  
### <a name="httphelppageurl"></a>HttpHelpPageUrl  
 数据类型：String  
  
 访问类型：只读  
  
 设置位置，在该位置发布服务 WSDL 以便使用 HTTP 进行检索。  
  
### <a name="httpshelppageenabled"></a>HttpsHelpPageEnabled  
 数据类型：Boolean  
  
 访问类型：只读  
  
 控制服务是否在 `HttpsGetUrl` 属性控制的地址上通过 HTTPS 发布其 WSDL。  
  
### <a name="httpshelppageurl"></a>HttpsHelpPageUrl  
 数据类型：String  
  
 访问类型：只读  
  
 设置位置，在该位置发布服务 WSDL 以便使用 HTTPS 进行检索。  
  
### <a name="includeexceptiondetailinfaults"></a>IncludeExceptionDetailInFaults  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定是否在返回给客户端的 SOAP 错误详细信息中包含托管异常信息以供调试。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅  
 <xref:System.ServiceModel.Description.ServiceDebugBehavior>
