---
title: Metodo ICorDebugMetaDataLocator::GetMetaData
ms.date: 03/30/2017
api_name:
- ICorDebugMetaDataLocator.GetMetaData
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugMetaDataLocator::GetMetaData
helpviewer_keywords:
- ICorDebugMetaDataLocator::GetMetaData method [.NET Framework debugging]
- GetMetaData method, ICorDebugMetaDataLocator interface [.NET Framework debugging]
ms.assetid: f9b0ff22-54db-45eb-9cc3-508000a3141d
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c29e581a77ac90882d102cfee2c715e9c309e1a3
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61780898"
---
# <a name="icordebugmetadatalocatorgetmetadata-method"></a>Metodo ICorDebugMetaDataLocator::GetMetaData
Chiede al debugger di restituire il percorso completo di un modulo i cui metadati sono necessari per completare un'operazione richiesta dal debugger.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT GetMetaData(  
      [in] LPCWSTR wszImagePath,  
      [in] DWORD   dwImageTimeStamp,  
      [in] DWORD   dwImageSize,  
      [in] ULONG32 cchPathBuffer,  
      [out] ULONG32 * pcchPathBuffer,  
      [out, size_is(cchPathBuffer), length_is(*pcchPathBuffer)]  
               WCHAR wszPathBuffer[]  
      );  
```  
  
## <a name="parameters"></a>Parametri  
 `wszImagePath`  
 [in] Stringa con terminazione null che rappresenta il percorso completo del file. Se il percorso completo non è disponibile, il nome e l'estensione del file (*nomefile*. *estensione*).  
  
 `dwImageTimeStamp`  
 [in] Timestamp dalle intestazioni del file PE dell'immagine. Questo parametro può essere potenzialmente usato per un server di simboli ([SymSrv](/windows/desktop/debug/using-symsrv)) ricerca.  
  
 `dwImageSize`  
 [in] Dimensioni dell'immagine dalle intestazioni del file PE. Questo parametro può essere potenzialmente usato per una ricerca in SymSrv.  
  
 `cchPathBuffer`  
 [in] Numero di caratteri in `wszPathBuffer`.  
  
 `pcchPathBuffer`  
 [out] Numero di `WCHAR` scritto in `wszPathBuffer`.  
  
 Se il metodo restituisce E_NOT_SUFFICIENT_BUFFER, contiene il numero di `WCHAR` necessario per archiviare il percorso.  
  
 `wszPathBuffer`  
 [out] Puntatore a un buffer in cui il debugger copierà il percorso completo del file che contiene i metadati richiesti.  
  
 Il `ofReadOnly` flag dal [CorOpenFlags](../../../../docs/framework/unmanaged-api/metadata/coropenflags-enumeration.md) enumerazione viene utilizzata per richiedere l'accesso di sola lettura ai metadati in questo file.  
  
## <a name="return-value"></a>Valore restituito  
 Questo metodo restituisce gli specifici HRESULT seguenti, nonché gli errori di HRESULT che indicano la mancata riuscita del metodo. Tutti gli altri HRESULT di errore indicano che il file non è recuperabile.  
  
|HRESULT|Descrizione|  
|-------------|-----------------|  
|S_OK|Metodo completato correttamente. `wszPathBuffer` contiene il percorso completo del file ed è con terminazione null.|  
|E_NOT_SUFFICIENT_BUFFER|Le dimensioni correnti di `wszPathBuffer` non sono sufficienti a contenere il percorso completo. In questo caso, `pcchPathBuffer` contiene il numero necessario di `WCHAR`s, incluso il carattere di terminazione null e `GetMetaData` viene chiamato una seconda volta con le dimensioni del buffer richiesto.|  
  
## <a name="remarks"></a>Note  
 Se `wszImagePath` contiene un percorso completo di un modulo da un dump, specifica il percorso dal computer in cui è stato recuperato il dump. Il file può non esistere in questa posizione oppure un file errato con lo stesso nome può essere archiviato nel percorso.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** CorDebug.idl, CorDebug.h  
  
 **Libreria:** CorGuids.lib  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche

- [Interfaccia ICorDebugThread4](../../../../docs/framework/unmanaged-api/debugging/icordebugthread4-interface.md)
- [Interfacce di debug](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
- [Debug](../../../../docs/framework/unmanaged-api/debugging/index.md)
