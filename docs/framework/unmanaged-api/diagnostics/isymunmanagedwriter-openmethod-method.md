---
title: Metodo ISymUnmanagedWriter::OpenMethod
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.OpenMethod
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::OpenMethod
helpviewer_keywords:
- ISymUnmanagedWriter::OpenMethod method [.NET Framework debugging]
- OpenMethod method [.NET Framework debugging]
ms.assetid: fb90cb7f-af88-45e8-a99f-80a0bbddb08b
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: b05ba185a9ad4ab076d29d7d609734d41677b760
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61986050"
---
# <a name="isymunmanagedwriteropenmethod-method"></a>Metodo ISymUnmanagedWriter::OpenMethod
Apre un metodo nel quale simbolo vengono create le informazioni. Il metodo specificato diventa il metodo corrente per le chiamate definire punti di sequenza, parametri e gli ambiti lessicali. È un ambito lessicale implicito l'intero metodo. La riapertura di un metodo che era stato precedentemente chiuso Cancella qualsiasi simboli definiti in precedenza per tale metodo. Può esistere un solo metodo open alla volta.  
  
## <a name="syntax"></a>Sintassi  
  
```  
HRESULT OpenMethod(  
    [in] mdMethodDef method);  
```  
  
## <a name="parameters"></a>Parametri  
 `method`  
 [in] Il token di metadati per il metodo da aprire.  
  
## <a name="return-value"></a>Valore restituito  
 S_OK se il metodo ha esito positivo; in caso contrario, E_FAIL o qualche altro codice di errore.  
  
## <a name="requirements"></a>Requisiti  
 **Intestazione:** CorSym.idl, CorSym.h  
  
## <a name="see-also"></a>Vedere anche

- [Interfaccia ISymUnmanagedWriter](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-interface.md)
- [Metodo CloseMethod](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-closemethod-method.md)
- [Metodo OpenMethod2](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter3-openmethod2-method.md)
