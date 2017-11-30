---
title: "IHostIoCompletionManager::GetMaxThreads 方法"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: IHostIoCompletionManager.GetMaxThreads
api_location: mscoree.dll
api_type: COM
f1_keywords: IHostIoCompletionManager::GetMaxThreads
helpviewer_keywords:
- IHostIoCompletionManager::GetMaxThreads method [.NET Framework hosting]
- GetMaxThreads method, IHostIoCompletionManager interface [.NET Framework hosting]
ms.assetid: e7a6cadc-2433-4472-a701-58891abcde45
topic_type: apiref
caps.latest.revision: "9"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 4e7df4acd435c767a1d0c3ae4484a236359cfb38
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2017
---
# <a name="ihostiocompletionmanagergetmaxthreads-method"></a><span data-ttu-id="18e8f-102">IHostIoCompletionManager::GetMaxThreads 方法</span><span class="sxs-lookup"><span data-stu-id="18e8f-102">IHostIoCompletionManager::GetMaxThreads Method</span></span>
<span data-ttu-id="18e8f-103">获取服务输入/输出请求的最大主机可以分配的线程数。</span><span class="sxs-lookup"><span data-stu-id="18e8f-103">Gets the maximum number of threads that the host can allot to service I/O requests.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="18e8f-104">语法</span><span class="sxs-lookup"><span data-stu-id="18e8f-104">Syntax</span></span>  
  
```  
HRESULT GetMaxThreads (  
    [out] DWORD *pdwMaxIoCompletionThreads  
);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="18e8f-105">参数</span><span class="sxs-lookup"><span data-stu-id="18e8f-105">Parameters</span></span>  
 `pdwMaxIoCompletionThreads`  
 <span data-ttu-id="18e8f-106">[out]最大主机可以分配给服务输入/输出请求线程池中的线程数指向的指针。</span><span class="sxs-lookup"><span data-stu-id="18e8f-106">[out] A pointer to the maximum number of threads in the thread pool that the host can allot to service I/O requests.</span></span>  
  
## <a name="return-value"></a><span data-ttu-id="18e8f-107">返回值</span><span class="sxs-lookup"><span data-stu-id="18e8f-107">Return Value</span></span>  
  
|<span data-ttu-id="18e8f-108">HRESULT</span><span class="sxs-lookup"><span data-stu-id="18e8f-108">HRESULT</span></span>|<span data-ttu-id="18e8f-109">描述</span><span class="sxs-lookup"><span data-stu-id="18e8f-109">Description</span></span>|  
|-------------|-----------------|  
|<span data-ttu-id="18e8f-110">S_OK</span><span class="sxs-lookup"><span data-stu-id="18e8f-110">S_OK</span></span>|<span data-ttu-id="18e8f-111">`GetMaxThreads`已成功返回。</span><span class="sxs-lookup"><span data-stu-id="18e8f-111">`GetMaxThreads` returned successfully.</span></span>|  
|<span data-ttu-id="18e8f-112">HOST_E_CLRNOTAVAILABLE</span><span class="sxs-lookup"><span data-stu-id="18e8f-112">HOST_E_CLRNOTAVAILABLE</span></span>|<span data-ttu-id="18e8f-113">公共语言运行时 (CLR) 尚未加载到进程中，或 CLR 处于不能运行托管的代码或成功处理调用的状态。</span><span class="sxs-lookup"><span data-stu-id="18e8f-113">The common language runtime (CLR) has not been loaded into a process, or the CLR is in a state in which it cannot run managed code or process the call successfully.</span></span>|  
|<span data-ttu-id="18e8f-114">HOST_E_TIMEOUT</span><span class="sxs-lookup"><span data-stu-id="18e8f-114">HOST_E_TIMEOUT</span></span>|<span data-ttu-id="18e8f-115">调用操作已超时。</span><span class="sxs-lookup"><span data-stu-id="18e8f-115">The call timed out.</span></span>|  
|<span data-ttu-id="18e8f-116">HOST_E_NOT_OWNER</span><span class="sxs-lookup"><span data-stu-id="18e8f-116">HOST_E_NOT_OWNER</span></span>|<span data-ttu-id="18e8f-117">调用方不拥有该锁。</span><span class="sxs-lookup"><span data-stu-id="18e8f-117">The caller does not own the lock.</span></span>|  
|<span data-ttu-id="18e8f-118">HOST_E_ABANDONED</span><span class="sxs-lookup"><span data-stu-id="18e8f-118">HOST_E_ABANDONED</span></span>|<span data-ttu-id="18e8f-119">事件已被取消时被阻塞的线程，或者纤程正在等待它。</span><span class="sxs-lookup"><span data-stu-id="18e8f-119">An event was canceled while a blocked thread or fiber was waiting on it.</span></span>|  
|<span data-ttu-id="18e8f-120">E_FAIL</span><span class="sxs-lookup"><span data-stu-id="18e8f-120">E_FAIL</span></span>|<span data-ttu-id="18e8f-121">出现未知的灾难性故障。</span><span class="sxs-lookup"><span data-stu-id="18e8f-121">An unknown catastrophic failure occurred.</span></span> <span data-ttu-id="18e8f-122">如果某方法返回 E_FAIL，CLR 不再可用进程内。</span><span class="sxs-lookup"><span data-stu-id="18e8f-122">When a method returns E_FAIL, the CLR is no longer usable within the process.</span></span> <span data-ttu-id="18e8f-123">到托管方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。</span><span class="sxs-lookup"><span data-stu-id="18e8f-123">Subsequent calls to hosting methods return HOST_E_CLRNOTAVAILABLE.</span></span>|  
|<span data-ttu-id="18e8f-124">E_NOTIMPL</span><span class="sxs-lookup"><span data-stu-id="18e8f-124">E_NOTIMPL</span></span>|<span data-ttu-id="18e8f-125">主机未提供的实现`GetMaxThreads`。</span><span class="sxs-lookup"><span data-stu-id="18e8f-125">The host does not provide an implementation of `GetMaxThreads`.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="18e8f-126">备注</span><span class="sxs-lookup"><span data-stu-id="18e8f-126">Remarks</span></span>  
 <span data-ttu-id="18e8f-127">主机可能想对可以分配给来处理 I/O 请求，原因例如实现、 性能或可伸缩性的线程数的独有控制。</span><span class="sxs-lookup"><span data-stu-id="18e8f-127">A host might want exclusive control over the number of threads that can be allotted to process I/O requests, for reasons such as implementation, performance, or scalability.</span></span> <span data-ttu-id="18e8f-128">主机不为此，需要实现`GetMaxThreads`。</span><span class="sxs-lookup"><span data-stu-id="18e8f-128">For this reason, the host is not required to implement `GetMaxThreads`.</span></span> <span data-ttu-id="18e8f-129">在这种情况下，主机应通过此方法返回 E_NOTIMPL。</span><span class="sxs-lookup"><span data-stu-id="18e8f-129">In this case, the host should return E_NOTIMPL from this method.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="18e8f-130">要求</span><span class="sxs-lookup"><span data-stu-id="18e8f-130">Requirements</span></span>  
 <span data-ttu-id="18e8f-131">**平台：**请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="18e8f-131">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="18e8f-132">**标头：** MSCorEE.h</span><span class="sxs-lookup"><span data-stu-id="18e8f-132">**Header:** MSCorEE.h</span></span>  
  
 <span data-ttu-id="18e8f-133">**库：**作为 MSCorEE.dll 中的资源</span><span class="sxs-lookup"><span data-stu-id="18e8f-133">**Library:** Included as a resource in MSCorEE.dll</span></span>  
  
 <span data-ttu-id="18e8f-134">**.NET framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="18e8f-134">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="18e8f-135">另请参阅</span><span class="sxs-lookup"><span data-stu-id="18e8f-135">See Also</span></span>  
 [<span data-ttu-id="18e8f-136">ICLRIoCompletionManager 接口</span><span class="sxs-lookup"><span data-stu-id="18e8f-136">ICLRIoCompletionManager Interface</span></span>](../../../../docs/framework/unmanaged-api/hosting/iclriocompletionmanager-interface.md)  
 [<span data-ttu-id="18e8f-137">IHostIoCompletionManager 接口</span><span class="sxs-lookup"><span data-stu-id="18e8f-137">IHostIoCompletionManager Interface</span></span>](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-interface.md)