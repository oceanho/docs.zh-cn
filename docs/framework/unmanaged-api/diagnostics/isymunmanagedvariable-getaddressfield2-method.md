---
title: "ISymUnmanagedVariable::GetAddressField2 方法"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ISymUnmanagedVariable.GetAddressField2
api_location: diasymreader.dll
api_type: COM
f1_keywords: ISymUnmanagedVariable::GetAddressField2
helpviewer_keywords:
- GetAddressField2 method [.NET Framework debugging]
- ISymUnmanagedVariable::GetAddressField2 method [.NET Framework debugging]
ms.assetid: 1f25b294-72b6-4882-a49b-6c9d364b6008
topic_type: apiref
caps.latest.revision: "9"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: d3e9124cb800416c44596d7b6f21a21c416e0e94
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="isymunmanagedvariablegetaddressfield2-method"></a>ISymUnmanagedVariable::GetAddressField2 方法
获取此变量的第二个地址字段。 其含义取决于类型的地址。  
  
## <a name="syntax"></a>语法  
  
```  
HRESULT GetAddressField2(  
    [out, retval] ULONG32* pRetVal);  
```  
  
#### <a name="parameters"></a>参数  
 `pRetVal`  
 [out]指向的指针`ULONG32`接收第二个地址字段。  
  
## <a name="return-value"></a>返回值  
 如果该方法成功; 则为 S_OK否则为 E_FAIL 或某些其他错误代码。  
  
## <a name="requirements"></a>要求  
 **标头：** CorSym.idl、 CorSym.h  
  
## <a name="see-also"></a>另请参阅  
 [ISymUnmanagedVariable 接口](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-interface.md)  
 [GetAddressField1 方法](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-getaddressfield1-method.md)  
 [GetAddressField3 方法](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-getaddressfield3-method.md)  
 [GetAddressKind 方法](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedvariable-getaddresskind-method.md)
