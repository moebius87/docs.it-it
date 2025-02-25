---
title: Metodo ICorDebugController::HasQueuedCallbacks
ms.date: 03/30/2017
api_name:
- ICorDebugController.HasQueuedCallbacks
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::HasQueuedCallbacks
helpviewer_keywords:
- HasQueuedCallbacks method [.NET Framework debugging]
- ICorDebugController::HasQueuedCallbacks method [.NET Framework debugging]
ms.assetid: 0d6a1cd9-370b-4462-adbf-e3980e897ea7
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: ce6ad24e5e670db21d3a6942ab4650a68ae44568
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61749553"
---
# <a name="icordebugcontrollerhasqueuedcallbacks-method"></a>Metodo ICorDebugController::HasQueuedCallbacks
Ottiene un valore che indica se i callback gestiti attualmente in coda per il thread specificato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT HasQueuedCallbacks (  
    [in] ICorDebugThread *pThread,  
    [out] BOOL           *pbQueued  
);  
```  
  
## <a name="parameters"></a>Parametri  
 `pThread`  
 [in] Un puntatore a un oggetto "ICorDebugThread" che rappresenta il thread.  
  
 `pbQueued`  
 [out] Un puntatore a un valore che rappresenta `true` se i callback gestiti sono attualmente in coda per il thread specificato; in caso contrario, `false`.  
  
 Se si specifica null per il `pThread` parametro, `HasQueuedCallbacks` restituirà `true` se sono presenti attualmente callback gestito accodati per uno o più thread.  
  
## <a name="remarks"></a>Note  
 I callback saranno inviati uno alla volta, ogni volta che [ICorDebugController](../../../../docs/framework/unmanaged-api/debugging/icordebugcontroller-continue-method.md) viene chiamato. Il debugger può controllare questo flag se si desiderano segnalare gli eventi di debug più che si verificano contemporaneamente.  
  
 Quando si trovano nella coda degli eventi di debug, essi essersi già verificati, in modo che il debugger è necessario eliminare l'intera coda per essere certi dello stato dell'oggetto del debug. (Chiamare `ICorDebugController::Continue` per svuotare la coda.) Ad esempio, se la coda contiene due eventi di debug sul thread *X*, e il debugger sospende il thread *X* dopo il primo evento di debug e quindi chiama `ICorDebugController::Continue`, il secondo evento di debug per thread *X* verranno inviati anche se il thread è stata sospesa.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** CorDebug.idl, CorDebug.h  
  
 **Libreria:** CorGuids.lib  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche
