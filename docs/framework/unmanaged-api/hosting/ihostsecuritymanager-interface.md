---
title: Interfaccia IHostSecurityManager
ms.date: 03/30/2017
api_name:
- IHostSecurityManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager
helpviewer_keywords:
- IHostSecurityManager interface [.NET Framework hosting]
ms.assetid: c3be2cbd-2d93-438b-9888-9a0251b63c03
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: a2c71f32dfd190e188bb28aad5d51c72160eb4bc
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64603248"
---
# <a name="ihostsecuritymanager-interface"></a>Interfaccia IHostSecurityManager
Fornisce metodi che consentono l'accesso e controllo sul contesto di sicurezza del thread attualmente in esecuzione.  
  
## <a name="methods"></a>Metodi  
  
|Metodo|Descrizione|  
|------------|-----------------|  
|[Metodo GetSecurityContext](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-getsecuritycontext-method.md)|Ottiene l'oggetto richiesto [IHostSecurityContext](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritycontext-interface.md) dall'host.|  
|[Metodo ImpersonateLoggedOnUser](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-impersonateloggedonuser-method.md)|Le richieste che codice eseguito con le credenziali dell'identità dell'utente corrente.|  
|[Metodo OpenThreadToken](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-openthreadtoken-method.md)|Apre il token di accesso discrezionale associato al thread corrente.|  
|[Metodo RevertToSelf](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-reverttoself-method.md)|Termina la rappresentazione dell'identità dell'utente corrente e restituisce il token del thread originale.|  
|[Metodo SetSecurityContext](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-setsecuritycontext-method.md)|Imposta il contesto di sicurezza per il thread attualmente in esecuzione.|  
|[Metodo SetThreadToken](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritymanager-setthreadtoken-method.md)|Imposta un handle per il thread attualmente in esecuzione.|  
  
## <a name="remarks"></a>Note  
 Un host può controllare l'accesso di codice ai token di thread dal common language runtime (CLR) e codice utente. Inoltre possibile garantire che la sicurezza completa le informazioni di contesto viene passate attraverso operazioni asincrone o punti di codice con l'accesso al codice con restrizioni. `IHostSecurityContext` incapsula questo informazioni sul contesto di sicurezza, è opache al CLR.  
  
 Common Language Runtime gestisce il contesto di thread gestiti internamente. Viene eseguita una query, specifico del processo `IHostSecurityManager` nelle situazioni seguenti:  
  
- Nel thread finalizzatore, durante l'esecuzione del finalizzatore.  
  
- Durante l'esecuzione dei costruttori di classe e modulo.  
  
- In corrispondenza di punti asincroni sul thread di lavoro, nelle chiamate al [IHostThreadPoolManager](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-queueuserworkitem-method.md) (metodo).  
  
- Nella manutenzione di porte di completamento i/o.  
  
## <a name="requirements"></a>Requisiti  
 **Piattaforme:** Vedere [Requisiti di sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Intestazione:** MSCorEE.h  
  
 **Libreria:** Inclusa come risorsa in Mscoree. dll  
  
 **Versioni di .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Vedere anche

- [Interfaccia IHostSecurityContext](../../../../docs/framework/unmanaged-api/hosting/ihostsecuritycontext-interface.md)
- [Interfacce di hosting](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
