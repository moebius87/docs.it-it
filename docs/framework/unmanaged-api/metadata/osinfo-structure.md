---
title: Struttura OSINFO
ms.date: 03/30/2017
api_name:
- OSINFO
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- OSINFO
helpviewer_keywords:
- OSINFO structure [.NET Framework metadata]
ms.assetid: fac7b480-7adb-4450-a5e9-690fed81ffae
topic_type:
- apiref
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 0aba49fb4a60b2e471c541a8d8531a1cbc8627f9
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61775200"
---
# <a name="osinfo-structure"></a>Struttura OSINFO
Contiene i dettagli sul sistema operativo per un assembly o un modulo.  
  
## <a name="syntax"></a>Sintassi  
  
```  
typedef struct {  
    DWORD   dwOSPlatformId;  
    DWORD   dwOSMajorVersion;   
    DWORD   dwOSMinorVersion;   
} OSINFO;  
```  
  
## <a name="members"></a>Membri  
  
|Member|Descrizione|  
|------------|-----------------|  
|`dwOSPlatformId`|Uno dei valori identificatore definiti dalla funzione della piattaforma Microsoft Windows `GetVersionEx`. Sono supportati i valori seguenti:<br /><br /> -VER_PLATFORM_WIN32s, o a 0x0000, specificare Microsoft Windows 3.1.<br />-VER_PLATFORM_WIN32_WINDOWS, o 0x0001, per specificare Windows 95, Windows 98 o discendenti da essi i sistemi operativi.<br />-VER_PLATFORM_WIN32_NT, o 0x0010, per specificare i sistemi operativi o Windows NT che derivano da essa.|  
|`dwOSMajorVersion`|La versione principale del sistema operativo o un valore NULL per indicare tutte le versioni.|  
|`dwOSMinorVersion`|La versione secondaria del sistema operativo o un valore NULL per indicare tutte le versioni.|  
  
## <a name="remarks"></a>Note  
 `OSINFO` basa il `OSVERSIONINFOEX` struttura usati nelle chiamate alla funzione della piattaforma Microsoft Windows `GetVersionEx`. Questa struttura viene utilizzata dalla struttura ASSEMBLYMETADATA per indicare il supporto del sistema operativo.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** Cor. h  
  
 **Libreria:** Usato come risorsa in Mscoree. dll  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche

- [Strutture di metadati](../../../../docs/framework/unmanaged-api/metadata/metadata-structures.md)
- [Interfaccia IMetaDataAssemblyEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)
