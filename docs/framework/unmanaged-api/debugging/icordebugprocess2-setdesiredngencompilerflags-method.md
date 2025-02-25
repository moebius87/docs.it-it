---
title: Metodo ICorDebugProcess2::SetDesiredNGENCompilerFlags
ms.date: 03/30/2017
api_name:
- ICorDebugProcess2.SetDesiredNGENCompilerFlags
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess2::SetDesiredNGENCompilerFlags
helpviewer_keywords:
- ICorDebugProcess2::SetDesiredNGENCompilerFlags method [.NET Framework debugging]
- SetDesiredNGENCompilerFlags method [.NET Framework debugging]
ms.assetid: 98320175-7c5e-4dbb-8683-86fa82e2641f
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: e061c3f3dc95e63339d6fd5f82b3cb4d38a4b6c6
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61948824"
---
# <a name="icordebugprocess2setdesiredngencompilerflags-method"></a>Metodo ICorDebugProcess2::SetDesiredNGENCompilerFlags
Imposta i flag che devono essere incorporati in un'immagine precompilata affinché il runtime di caricare tale immagine nel processo corrente.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT SetDesiredNGENCompilerFlags (  
    [in] DWORD    pdwFlags  
);  
```  
  
## <a name="parameters"></a>Parametri  
 `pdwFlags`  
 [in] Valore di [CorDebugJITCompilerFlags](../../../../docs/framework/unmanaged-api/debugging/cordebugjitcompilerflags-enumeration.md) enumerazione che specifica i flag del compilatore usato per selezionare l'immagine di pre-compilata corretta.  
  
## <a name="remarks"></a>Note  
 Il `SetDesiredNGENCompilerFlags` metodo specifica i flag che devono essere incorporati in un'immagine precompilata in modo che il runtime caricherà l'immagine in questo processo. I flag impostati da questo metodo vengono utilizzati solo per selezionare l'immagine corretta precompilata. Se l'immagine non esiste, il runtime caricherà invece l'immagine di Microsoft intermediate language (MSIL) e il compilatore JIT just-in-time. In tal caso, il debugger deve ancora utilizzare il [ICorDebugModule2::SetJITCompilerFlags](../../../../docs/framework/unmanaged-api/debugging/icordebugmodule2-setjitcompilerflags-method.md) metodo per impostare i flag desiderati per la compilazione JIT.  
  
 Se viene caricata un'immagine, ma alcuni la compilazione JIT deve essere eseguita per quell'immagine (che sarà il caso se l'immagine contiene generics), il flag del compilatore specificato da di `SetDesiredNGENCompilerFlags` metodo verrà applicate a compilazione JIT aggiuntiva.  
  
 Il `SetDesiredNGENCompilerFlags` metodo deve essere chiamato durante il [ICorDebugManagedCallback::](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-createprocess-method.md) callback. Tenta di chiamare il `SetDesiredNGENCompilerFlags` metodo in un secondo momento avrà esito negativo. Inoltre, i tentativi di impostare i flag che non sono definite nel `CorDebugJITCompilerFlags` enumerazione o che non sono consentiti per il processo specificato non riuscirà.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** CorDebug.idl, CorDebug.h  
  
 **Libreria:** CorGuids.lib  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche

- [Interfaccia ICorDebug](../../../../docs/framework/unmanaged-api/debugging/icordebug-interface.md)
- [Interfaccia ICorDebugManagedCallback](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-interface.md)
