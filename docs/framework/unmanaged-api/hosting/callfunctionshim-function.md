---
title: Funzione CallFunctionShim
ms.date: 03/30/2017
api_name:
- CallFunctionShim
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- CallFunctionShim
helpviewer_keywords:
- CallfunctionShim function [.NET Framework hosting]
ms.assetid: 37118465-ddf3-41f0-bf27-335b72777e63
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 3dfe2c98fd5898a0ad5a1d4fd9e89c7f20741bb0
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66490689"
---
# <a name="callfunctionshim-function"></a>Funzione CallFunctionShim
Effettua una chiamata alla funzione che ha il nome specificato e i parametri nella libreria specificata.  
  
 Questa funzione è stata deprecata in .NET Framework 4.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT CallFunctionShim (  
    [in] LPCWSTR     szDllName,  
    [in] LPCSTR      szFunctionName,  
    [in] LPVOID      lpvArgument1,  
    [in] LPVOID      lpvArgument2,  
    [in] LPCWSTR     szVersion,  
    [in] LPVOID      pvReserved  
);  
```  
  
## <a name="parameters"></a>Parametri  
 `szDllName`  
 [in] Il nome della libreria che contiene la funzione.  
  
 `szFunctionName`  
 [in] Il nome della funzione.  
  
 `lpvArgument1`  
 [in] Il primo argomento da passare alla funzione.  
  
 `lpvArgument2`  
 [in] Il secondo argomento da passare alla funzione.  
  
 `szVersion`  
 [in] La versione della libreria che contiene la funzione.  
  
 `pvReserved`  
 [in] Riservato per utilizzi futuri. Passare zero in questo parametro.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** MSCorEE.h  
  
 **Libreria:** MSCorEE.dll  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche

- [Funzioni di hosting CLR deprecate](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
