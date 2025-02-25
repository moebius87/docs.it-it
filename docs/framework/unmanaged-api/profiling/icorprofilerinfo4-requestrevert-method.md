---
title: Metodo ICorProfilerInfo4::RequestRevert
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.RequestRevert
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::RequestRevert
helpviewer_keywords:
- RequestRevert method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::RequestRevert method [.NET Framework profiling]
ms.assetid: 70261da5-5933-4e25-9de0-ddf51cba56cc
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 92137e1a5b0923bc34745513715934c483616700
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62000493"
---
# <a name="icorprofilerinfo4requestrevert-method"></a>Metodo ICorProfilerInfo4::RequestRevert
Ripristina tutte le istanze delle funzioni specificate alle versioni originali.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT RequestRevert (  
   [in] ULONG    cFunctions,  
   [in, size_is(cFunctions)]  ModuleID    moduleIds[],  
   [in, size_is(cFunctions)]  mdMethodDef methodIds[],  
   [out, size_is(cFunctions)]  HRESULT status[]);  
```  
  
## <a name="parameters"></a>Parametri  
 `cFunctions`  
 [in] Numero delle funzioni da ripristinare.  
  
 `moduleIds`  
 [in] Specifica la parte `moduleId` delle coppie (`module`, `methodDef`) che identificano le funzioni da ripristinare.  
  
 `methodIds`  
 [in] Specifica la parte `methodId` delle coppie (`module`, `methodDef`) che identificano le funzioni da ripristinare.  
  
 `status`  
 [out] Matrice di HRESULT riportati nella sezione "HRESULT di stato" più avanti in questo argomento. Ciascun HRESULT indica la riuscita o la mancata riuscita del ripristino di ogni funzione specificata nelle matrici `moduleIds` e `methodIds` parallele.  
  
## <a name="return-value"></a>Valore restituito  
 Questo metodo restituisce gli specifici HRESULT seguenti, nonché gli errori di HRESULT che indicano la mancata riuscita del metodo.  
  
|HRESULT|Descrizione|  
|-------------|-----------------|  
|S_OK|È stato effettuato un tentativo di ripristinare tutte le richieste. Tuttavia, la matrice di stato restituito deve essere verificata per determinare quali funzioni sono state annullate correttamente.|  
|CORPROF_E_CALLBACK4_REQUIRED|Il profiler deve implementare il [ICorProfilerCallback4](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback4-interface.md) interfaccia per la chiamata a essere supportato.|  
|CORPROF_E_REJIT_NOT_ENABLED|La ricompilazione JIT non è stata abilitata. È necessario abilitare la ricompilazione JIT durante l'inizializzazione usando il [ICorProfilerInfo:: SetEventMask](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo-seteventmask-method.md) metodo per impostare il `COR_PRF_ENABLE_REJIT` flag.|  
|E_INVALIDARG|Il parametro `cFunctions` è pari a 0 oppure `moduleIds` o `methodIds` è `NULL`.|  
|E_OUTOFMEMORY|CLR non è stato in grado di completare la richiesta a causa di memoria insufficiente.|  
  
## <a name="status-hresults"></a>HRESULT di stato  
  
|HRESULT matrice di stato|Descrizione|  
|--------------------------|-----------------|  
|S_OK|La funzione corrispondente è stata ripristinata.|  
|E_INVALIDARG|Il parametro `moduleID` o il parametro `methodDef` è `NULL`.|  
|CORPROF_E_DATAINCOMPLETE|Il modulo non è ancora completamente caricato o è in fase di scaricamento.|  
|CORPROF_E_MODULE_IS_DYNAMIC|Il modulo specificato è stato generato dinamicamente (ad esempio da `Reflection.Emit`). Di conseguenza, non è supportato da questo metodo.|  
|CORPROF_E_ACTIVE_REJIT_REQUEST_NOT_FOUND|CLR non può ripristinare la funzione specificata, perché non è stata trovata una richiesta di ricompilazione attiva corrispondente. La ricompilazione non è stata mai richiesta oppure la funzione era già stata ripristinata.|  
|Altro|Il sistema operativo ha restituito un errore esterno al controllo di CLR. Ad esempio, se una chiamata al sistema per modificare la sicurezza di accesso di una pagina di memoria non riesce, viene visualizzato un errore del sistema operativo.|  
  
## <a name="remarks"></a>Note  
 La volta successiva che verranno chiamate tutte le istanze di funzione ripristinate, verranno eseguite le versioni originali delle funzioni. Se una funzione è già in esecuzione, verrà terminata l'esecuzione della versione in esecuzione.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** CorProf.idl, CorProf.h  
  
 **Libreria:** CorGuids.lib  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche

- [Interfaccia ICorProfilerInfo4](../../../../docs/framework/unmanaged-api/profiling/icorprofilerinfo4-interface.md)
- [Interfacce di profilatura](../../../../docs/framework/unmanaged-api/profiling/profiling-interfaces.md)
- [Profilatura](../../../../docs/framework/unmanaged-api/profiling/index.md)
