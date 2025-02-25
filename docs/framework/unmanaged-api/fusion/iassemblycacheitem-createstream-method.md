---
title: Metodo IAssemblyCacheItem::CreateStream
ms.date: 03/30/2017
api_name:
- IAssemblyCacheItem.CreateStream
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCacheItem::CreateStream
helpviewer_keywords:
- CreateStream method [.NET Framework fusion]
- IAssemblyCacheItem::CreateStream method [.NET Framework fusion]
ms.assetid: 697ab0f4-470c-4baa-a415-4a975c42d0d5
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: a98273307003485202d8c12d5c27fda04ff5a0ae
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65629887"
---
# <a name="iassemblycacheitemcreatestream-method"></a>Metodo IAssemblyCacheItem::CreateStream

Crea un flusso con il nome specificato e il formato.

## <a name="syntax"></a>Sintassi

```cpp
HRESULT CreateStream (
    [in]  DWORD dwFlags,
    [in]  LPCWSTR pszStreamName,
    [in]  DWORD dwFormat,
    [in]  DWORD dwFormatFlags,
    [out] IStream **ppIStream,
    [in, optional] ULARGE_INTEGER *puliMaxSize
);
```

## <a name="parameters"></a>Parametri

`dwFlags`\
[in] Flag definiti in Fusion.

`pszStreamName`\
[in] Il nome del flusso da creare.

`dwFormat`\
[in] Il formato del file deve essere trasmessa.

`dwFormatFlags`\
[in] Flag specifici del formato definito in Fusion.

`ppIStream`\
[out] Un puntatore all'indirizzo del valore restituito [IStream](/windows/desktop/api/objidl/nn-objidl-istream) istanza.

`puliMaxSize`\
[in, optional] La dimensione massima del flusso fa `ppIStream`.

## <a name="requirements"></a>Requisiti

**Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).

**Intestazione:** Fusion. h

**Versioni di .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>Vedere anche

- [Interfaccia IAssemblyCacheItem](iassemblycacheitem-interface.md)
