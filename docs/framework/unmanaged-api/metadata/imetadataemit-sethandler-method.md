---
title: "IMetaDataEmit::SetHandler 方法"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: IMetaDataEmit.SetHandler
api_location: mscoree.dll
api_type: COM
f1_keywords: IMetaDataEmit::SetHandler
helpviewer_keywords:
- IMetaDataEmit::SetHandler method [.NET Framework metadata]
- SetHandler method [.NET Framework metadata]
ms.assetid: c6c1aaaf-e2cd-407c-b73e-fbe6ffd83bb3
topic_type: apiref
caps.latest.revision: "12"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: d4a56db43f932a155b4251f019e39dc5640eb014
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="imetadataemitsethandler-method"></a>IMetaDataEmit::SetHandler 方法
设置指定所引用的方法`IUnknown`指针作为令牌重新映射通知回调。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT SetHandler (   
    [in]  IUnknown    *pUnk  
);  
```  
  
#### <a name="parameters"></a>参数  
 `pUnk`  
 [in]要注册的处理程序。  
  
## <a name="remarks"></a>备注  
 元数据引擎通过使用提供的方法来发送通知`SetHandler`，到编译器不以优化方式生成记录并希望保存的记录进行优化。  
  
 如果不通过提供的回调方法`SetHandler`，将执行不进行优化上保存除若干导入作用域已合并使用`IMapToken`上每个作用域的合并。  
  
## <a name="requirements"></a>要求  
 **平台：**请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头：** Cor.h  
  
 **库：**用作 MSCorEE.dll 中的资源  
  
 **.NET framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [IMetaDataEmit 接口](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)  
 [IMetaDataEmit2 接口](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
