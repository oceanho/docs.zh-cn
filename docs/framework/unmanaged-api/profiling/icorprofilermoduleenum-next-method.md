---
title: "ICorProfilerModuleEnum::Next 方法"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorProfilerModuleEnum.Next Method
api_location: mscorwks.dll
api_type: COM
f1_keywords: ICorProfilerModuleEnum::Next
helpviewer_keywords:
- ICorProfilerModuleEnum::Next method [.NET Framework profiling]
- Next method, ICorProfilerModuleEnum interface [.NET Framework profiling]
ms.assetid: a3cea59d-7622-4323-897a-0a464c40f77f
topic_type: apiref
caps.latest.revision: "13"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: c20b6970c0df30b75bacf76f52c7610bd4a3a5e7
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="icorprofilermoduleenumnext-method"></a><span data-ttu-id="0467c-102">ICorProfilerModuleEnum::Next 方法</span><span class="sxs-lookup"><span data-stu-id="0467c-102">ICorProfilerModuleEnum::Next Method</span></span>
<span data-ttu-id="0467c-103">从模块的序列集合中获取指定的连续模块数，从枚举器在该序列的当前位置开始。</span><span class="sxs-lookup"><span data-stu-id="0467c-103">Gets the specified number of contiguous modules from a sequential collection of modules, starting at the enumerator's current position in the sequence.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="0467c-104">语法</span><span class="sxs-lookup"><span data-stu-id="0467c-104">Syntax</span></span>  
  
```  
HRESULT Next([in]  ULONG      celt,  
             [out, size_is(celt), length_is(*pceltFetched)]  
                    ModuleID ids[],  
             [out] ULONG *   pceltFetched);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="0467c-105">参数</span><span class="sxs-lookup"><span data-stu-id="0467c-105">Parameters</span></span>  
 `celt`  
 <span data-ttu-id="0467c-106">[in] 要检索的模块的数量。</span><span class="sxs-lookup"><span data-stu-id="0467c-106">[in] The number of modules to retrieve.</span></span>  
  
 `ids`  
 <span data-ttu-id="0467c-107">[out] `ModuleID` 值的数组，其中每个表示检索的模块。</span><span class="sxs-lookup"><span data-stu-id="0467c-107">[out] An array of `ModuleID` values, each of which represents a retrieved module.</span></span>  
  
 `pceltFetched`  
 <span data-ttu-id="0467c-108">[out] 指向 `ids` 数组中实际返回的元素数目的指针。</span><span class="sxs-lookup"><span data-stu-id="0467c-108">[out] A pointer to the number of elements actually returned in the `ids` array.</span></span>  
  
## <a name="return-value"></a><span data-ttu-id="0467c-109">返回值</span><span class="sxs-lookup"><span data-stu-id="0467c-109">Return Value</span></span>  
 <span data-ttu-id="0467c-110">此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。</span><span class="sxs-lookup"><span data-stu-id="0467c-110">This method returns the following specific HRESULTs as well as HRESULT errors that indicate method failure.</span></span>  
  
|<span data-ttu-id="0467c-111">HRESULT</span><span class="sxs-lookup"><span data-stu-id="0467c-111">HRESULT</span></span>|<span data-ttu-id="0467c-112">描述</span><span class="sxs-lookup"><span data-stu-id="0467c-112">Description</span></span>|  
|-------------|-----------------|  
|<span data-ttu-id="0467c-113">S_OK</span><span class="sxs-lookup"><span data-stu-id="0467c-113">S_OK</span></span>|<span data-ttu-id="0467c-114">已返回 `celt` 元素。</span><span class="sxs-lookup"><span data-stu-id="0467c-114">`celt` elements were returned.</span></span>|  
|<span data-ttu-id="0467c-115">S_FALSE</span><span class="sxs-lookup"><span data-stu-id="0467c-115">S_FALSE</span></span>|<span data-ttu-id="0467c-116">返回的元素少于 `celt` 个，表示枚举已完成。</span><span class="sxs-lookup"><span data-stu-id="0467c-116">Fewer than `celt` elements were returned, which indicates that the enumeration is complete.</span></span>|  
  
## <a name="requirements"></a><span data-ttu-id="0467c-117">要求</span><span class="sxs-lookup"><span data-stu-id="0467c-117">Requirements</span></span>  
 <span data-ttu-id="0467c-118">**平台：**请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="0467c-118">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="0467c-119">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="0467c-119">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="0467c-120">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="0467c-120">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="0467c-121">**.NET framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="0467c-121">**.NET Framework Versions:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="0467c-122">另请参阅</span><span class="sxs-lookup"><span data-stu-id="0467c-122">See Also</span></span>  
 [<span data-ttu-id="0467c-123">ICorProfilerModuleEnum 接口</span><span class="sxs-lookup"><span data-stu-id="0467c-123">ICorProfilerModuleEnum Interface</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilermoduleenum-interface.md)  
 [<span data-ttu-id="0467c-124">分析接口</span><span class="sxs-lookup"><span data-stu-id="0467c-124">Profiling Interfaces</span></span>](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)