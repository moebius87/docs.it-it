---
title: Metodo ISOSDacInterface::GetMethodDescData
ms.date: 01/16/2019
api.name:
- ISOSDacInterface::GetMethodDescData Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- ISOSDacInterface::GetMethodDescData Method
helpviewer.keywords:
- ISOSDacInterface::GetMethodDescData Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: cdbf662c664d6c87b2fa17bcb10d735b0f573dd2
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65632308"
---
# <a name="isosdacinterfacegetmethoddescdata-method"></a>Metodo ISOSDacInterface::GetMethodDescData

Ottiene i dati per il puntatore MethodDesc specificato.

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>Sintassi

```
HRESULT GetMethodDescData(
    CLRDATA_ADDRESS            methodDesc,
    CLRDATA_ADDRESS            ip,
    DacpMethodDescData *data,
    ULONG                      cRevertedRejitVersions,
    DacpReJitData      *rgRevertedRejitData,
    void                      *pcNeededRevertedRejitData
);
```

## <a name="parameters"></a>Parametri

`methodDesc`\
[in] L'indirizzo del MethodDesc.

`ip`\
[in] L'indirizzo IP del metodo.

`data`\
[out] I dati associati con la MethodDesc come restituito dall'API interne.

`cRevertedRejitVersions`\
[out] Il numero di versioni rejit ripristinato.

`rgRevertedRejitData`\
[out] I dati associati con le versioni rejit ripristinato come restituito dall'API interne.

`pcNeededRevertedRejitData`\
[out] Il numero di byte necessari per archiviare i dati associati con le versioni ReJit ripristinate.

## <a name="remarks"></a>Note

Il metodo specificato fa parte di `ISOSDacInterface` interfaccia e corrisponde al 20 ° slot della tabella di metodo virtuale. Per poter utilizzarli [ `CLRDATA_ADDRESS` ](../common-data-types-unmanaged-api-reference.md) deve essere definito come un intero senza segno a 64 bit.

## <a name="requirements"></a>Requisiti

**Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
**Intestazione:** nessuno  
**Libreria:** nessuno  
**Versioni di .NET Framework:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>Vedere anche

- [Debug](index.md)
- [Interfaccia ISOSDacInterface](isosdacinterface-interface.md)
