---
title: Esecuzione side-by-side in-process
ms.date: 03/30/2017
helpviewer_keywords:
- in-process side-by-side execution
- side-by-side execution, in-process
ms.assetid: 18019342-a810-4986-8ec2-b933a17c2267
author: mairaw
ms.author: mairaw
ms.openlocfilehash: adf2e3e3d10f4f32952dbca270be4ca0924d0b73
ms.sourcegitcommit: 518e7634b86d3980ec7da5f8c308cc1054daedb7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2019
ms.locfileid: "66457272"
---
# <a name="in-process-side-by-side-execution"></a>Esecuzione side-by-side in-process
A partire da [!INCLUDE[net_v40_long](../../../includes/net-v40-long-md.md)], è possibile usare l'hosting side-by-side in-process per eseguire più versioni di Common Language Runtime (CLR) in un unico processo. Per impostazione predefinita, i componenti COM gestiti vengono eseguiti con la versione di .NET Framework con cui sono stati compilati, indipendentemente dalla versione di .NET Framework che viene caricata per il processo.  
  
## <a name="background"></a>Sfondo  
 .NET Framework ha sempre offerto l'hosting side-by-side per applicazioni di codice gestito, ma prima di .NET Framework 4 tale funzionalità non era disponibile per i componenti COM gestiti. In passato, i componenti COM gestiti che venivano caricati in un processo venivano eseguiti con la versione di runtime già caricata o con l'ultima versione di .NET Framework installata. Se tale versione non era compatibile con il componente COM, l'esecuzione di quest'ultimo aveva esito negativo.  
  
 .NET Framework 4 offre un nuovo approccio all'hosting side-by-side che assicura quanto segue:  
  
- L'installazione di una nuova versione di .NET Framework non ha alcun effetto sulle applicazioni esistenti.  
  
- Le applicazioni vengono eseguite con la versione di .NET Framework con cui sono state compilate. Non usano la nuova versione di .NET Framework a meno che non venga espressamente data istruzione in tal senso. Tuttavia, per le applicazioni è più facile passare all'uso di una nuova versione di .NET Framework.  
  
## <a name="effects-on-users-and-developers"></a>Effetti su utenti e sviluppatori  
  
- **Utenti finali e amministratori di sistema**. Tali utenti possono ora avere maggiore certezza che quando installano una nuova versione del runtime, in modo indipendente o con un'applicazione, ciò non avrà alcun impatto sui computer. Le applicazioni esistenti continueranno a essere eseguite come in precedenza.  
  
- **Sviluppatori di applicazioni**. L'hosting side-by-side non ha praticamente alcun effetto che possa interessare gli sviluppatori di applicazioni. Per impostazione predefinita, le applicazioni vengono sempre eseguite con la versione di .NET Framework con cui sono state compilate. Questo non è stato modificato. Tuttavia, gli sviluppatori possono eseguire l'override di questo comportamento e indirizzare l'applicazione per l'esecuzione in una versione più recente di .NET Framework. Vedere [Scenario 2](#scenarios).  
  
- **Sviluppatori di librerie e consumer**. L'hosting side-by-side non risolve i problemi di compatibilità che devono affrontare gli sviluppatori di librerie. Una libreria che viene caricata direttamente da un'applicazione, tramite un riferimento diretto o mediante una chiamata a <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>, continua a usare il runtime di <xref:System.AppDomain> in cui viene caricata. È consigliabile testare le librerie con tutte le versioni di .NET Framework che si vuole supportare. Se un'applicazione viene compilata usando il runtime di .NET Framework 4, ma include una libreria che è stata compilata usando un runtime precedente, tale libreria userà anche il runtime di .NET Framework 4. Tuttavia, se si ha un'applicazione compilata con un runtime precedente e una libreria compilata usando .NET Framework 4, è necessario forzare l'applicazione a usare anche .NET Framework 4. Vedere [scenario 3](#scenarios).  
  
- **Sviluppatori di componenti COM gestiti**. In passato, i componenti COM gestiti venivano eseguiti automaticamente usando la versione più recente del runtime installato nel computer. È ora possibile eseguire i componenti COM con la versione del runtime con cui sono stati compilati.  
  
     Come illustrato nella tabella seguente, i componenti compilati con .NET Framework versione 1.1 possono essere eseguiti side-by-side con i componenti della versione 4, ma non possono essere eseguiti con i componenti della versione 2.0, 3.0 o 3.5, perché l'hosting side-by-side non è disponibile per tali versioni.  
  
    |Versione di .NET Framework|1.1|2.0 - 3.5|4|  
    |----------------------------|---------|----------------|-------|  
    |1.1|Non applicabile|No|Sì|  
    |2.0 - 3.5|No|Non applicabile|Yes|  
    |4|Sì|Sì|Non applicabile|  
  
> [!NOTE]
>  Le versioni di .NET framework 3.0 e 3.5 sono state compilate in modo incrementale dalla versione 2.0 e non necessitano dell'esecuzione side-by-side. Sono praticamente la stessa versione.  
  
<a name="scenarios"></a>   
## <a name="common-side-by-side-hosting-scenarios"></a>Scenari comuni di hosting side-by-side  
  
- **Scenario 1:** applicazione nativa che usa i componenti COM compilati con versioni precedenti di .NET Framework.  
  
     Versioni di .NET Framework installate: .NET Framework 4 e tutte le altre versioni di .NET Framework usate dai componenti COM.  
  
     Cosa fare: in questo scenario, non eseguire alcuna operazione. I componenti COM verranno eseguiti con la versione di .NET Framework con la quale sono stati registrati.  
  
- **Scenario 2**: applicazione gestita compilata con [!INCLUDE[net_v20SP1_short](../../../includes/net-v20sp1-short-md.md)] che si preferirebbe eseguire con [!INCLUDE[dnprdnext](../../../includes/dnprdnext-md.md)], ma che si è disposti a eseguire in .NET Framework 4 se la versione 2.0 non è disponibile.  
  
     Versioni di .NET Framework installate: una versione precedente di .NET Framework e di .NET Framework 4.  
  
     Cosa fare: nel [file di configurazione dell'applicazione](../../../docs/framework/configure-apps/index.md) contenuto nella directory dell'applicazione usare [l'elemento \<startup>](../../../docs/framework/configure-apps/file-schema/startup/startup-element.md) e [l'elemento \<supportedRuntime>](../../../docs/framework/configure-apps/file-schema/startup/supportedruntime-element.md) impostati nel modo seguente:  
  
    ```xml  
    <configuration>  
      <startup >  
        <supportedRuntime version="v2.0.50727" />  
        <supportedRuntime version="v4.0" />  
      </startup>  
    </configuration>  
    ```  
  
- **Scenario 3:** applicazione nativa che usa componenti COM compilati con versioni precedenti di .NET Framework che si vuole eseguire con .NET Framework 4.  
  
     Versioni di .NET Framework installate: .NET Framework 4.  
  
     Cosa fare: nel file di configurazione dell'applicazione contenuto nella directory dell'applicazione usare l'elemento `<startup>` con l'attributo `useLegacyV2RuntimeActivationPolicy` impostato su `true` e l'elemento `<supportedRuntime>` impostato nel modo seguente:  
  
    ```xml  
    <configuration>  
      <startup useLegacyV2RuntimeActivationPolicy="true">  
        <supportedRuntime version="v4.0" />  
      </startup>  
    </configuration>  
    ```  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene illustrato un host COM non gestito che esegue un componente COM gestito usando la versione di .NET Framework con cui è stato compilato il componente da usare.  
  
 Per eseguire l'esempio riportato di seguito, compilare e registrare il seguente componente COM gestito utilizzando [!INCLUDE[net_v35_long](../../../includes/net-v35-long-md.md)]. Per registrare il componente, nel menu **Progetto** scegliere **Proprietà**, fare clic sulla scheda **Build**, quindi selezionare la casella di controllo **Registra per interoperabilità COM**.  
  
```csharp
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Runtime.InteropServices;  
  
namespace BasicComObject  
{  
    [ComVisible(true), Guid("9C99C4B5-CA54-4c58-8988-49B6811BA53B")]  
    public class MyObject : SimpleObjectModel.IPrintInfo  
    {  
        public MyObject()  
        {  
        }  
        public void PrintInfo()  
        {  
            Console.WriteLine("MyObject was activated in {0} runtime in:\n\tAppDomain {1}:{2}", System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion(), AppDomain.CurrentDomain.Id, AppDomain.CurrentDomain.FriendlyName);  
        }  
    }  
}  
```  
  
 Compilare la seguente applicazione C++ non gestita, che attiva l'oggetto COM che viene creato dall'esempio precedente.  
  
```cpp
#include "stdafx.h"  
#include <string>  
#include <iostream>  
#include <objbase.h>  
#include <string.h>  
#include <process.h>  
  
using namespace std;  
  
int _tmain(int argc, _TCHAR* argv[])  
{  
    char input;  
    CoInitialize(NULL) ;  
    CLSID clsid;  
    HRESULT hr;  
    HRESULT clsidhr = CLSIDFromString(L"{9C99C4B5-CA54-4c58-8988-49B6811BA53B}",&clsid);  
    hr = -1;  
    if (FAILED(clsidhr))  
    {  
        printf("Failed to construct CLSID from String\n");  
    }  
    UUID id = __uuidof(IUnknown);  
    IUnknown * pUnk = NULL;  
    hr = ::CoCreateInstance(clsid,NULL,CLSCTX_INPROC_SERVER,id,(void **) &pUnk);  
    if (FAILED(hr))  
    {  
        printf("Failed CoCreateInstance\n");  
    }else  
    {  
        pUnk->AddRef();  
        printf("Succeeded\n");  
    }  
  
    DISPID dispid;  
    IDispatch* pPrintInfo;  
    pUnk->QueryInterface(IID_IDispatch, (void**)&pPrintInfo);  
    OLECHAR FAR* szMethod[1];  
    szMethod[0]=OLESTR("PrintInfo");   
    hr = pPrintInfo->GetIDsOfNames(IID_NULL,szMethod, 1, LOCALE_SYSTEM_DEFAULT, &dispid);  
    DISPPARAMS dispparams;  
    dispparams.cNamedArgs = 0;  
    dispparams.cArgs = 0;  
    VARIANTARG* pvarg = NULL;  
    EXCEPINFO * pexcepinfo = NULL;  
    WORD wFlags = DISPATCH_METHOD ;  
;  
    LPVARIANT pvRet = NULL;  
    UINT * pnArgErr = NULL;  
    hr = pPrintInfo->Invoke(dispid,IID_NULL, LOCALE_USER_DEFAULT, wFlags,  
        &dispparams, pvRet, pexcepinfo, pnArgErr);  
    printf("Press Enter to exit");  
    scanf_s("%c",&input);  
    CoUninitialize();  
    return 0;  
}  
```  
  
## <a name="see-also"></a>Vedere anche

- [Elemento \<startup](../../../docs/framework/configure-apps/file-schema/startup/startup-element.md)
- [Elemento \<supportedRuntime>](../../../docs/framework/configure-apps/file-schema/startup/supportedruntime-element.md)
