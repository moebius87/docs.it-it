---
title: Interfaccia ICorDebugModule3
ms.date: 03/30/2017
api_name:
- ICorDebugModule3
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule3
helpviewer_keywords:
- ICorDebugModule3 interface [.NET Framework debugging]
ms.assetid: 0b69f945-263a-4e11-8512-89d27f6ea296
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: a340086be042c790ae7bf750759ff80f7c9eaf23
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61942441"
---
# <a name="icordebugmodule3-interface"></a>Interfaccia ICorDebugModule3
Crea un lettore di simboli per un modulo dinamico.  
  
## <a name="syntax"></a>Sintassi  
  
```  
interface ICorDebugModule3 : IUnknown  
{  
    HRESULT CreateReaderForInMemorySymbols  
      (  
      [in] REFIID riid,  
      [out][iid_is(riid)] void **  ppObj  
      );  
};  
```  
  
## <a name="methods"></a>Metodi  
  
|Metodo|Descrizione|  
|------------|-----------------|  
|[Metodo ICorDebugModule3::CreateReaderForInMemorySymbols](../../../../docs/framework/unmanaged-api/debugging/icordebugmodule3-createreaderforinmemorysymbols-method.md)|Crea un lettore di simboli (in genere [interfaccia di ISymUnmanagedReader](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedreader-interface.md)) per un modulo dinamico.|  
  
## <a name="remarks"></a>Note  
 Questa interfaccia estende logicamente le interfacce "ICorDebugModule" e "ICorDebugModule2".  
  
> [!NOTE]
>  Questa interfaccia non supporta la chiamata in modalità remota, tra computer o tra processi.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** CorDebug.idl, CorDebug.h  
  
 **Libreria:** CorGuids.lib  
  
 **Versioni di .NET framework:** 4.5, 4, 3.5 SP1
  
## <a name="see-also"></a>Vedere anche

- [Interfaccia ICorDebugRemoteTarget](../../../../docs/framework/unmanaged-api/debugging/icordebugremotetarget-interface.md)
- [Interfaccia ICorDebug](../../../../docs/framework/unmanaged-api/debugging/icordebug-interface.md)

- [Interfacce di debug](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
