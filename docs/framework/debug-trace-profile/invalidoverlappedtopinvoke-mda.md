---
title: MDA invalidOverlappedToPinvoke
ms.date: 03/30/2017
helpviewer_keywords:
- overlapped pointers
- managed debugging assistants (MDAs), overlapped pointers
- invalid overlapped pointer to platform invoke
- InvalidOverlappedToPinvoke MDA
- MDAs (managed debugging assistants), overlapped pointers
- pointers, overlapped
ms.assetid: 28876047-58bd-4fed-9452-c7da346d67c0
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 4bdb2035906b9383342201017b58d1d0050113b5
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61754492"
---
# <a name="invalidoverlappedtopinvoke-mda"></a>MDA invalidOverlappedToPinvoke
L'assistente al debug gestito `invalidOverlappedToPinvoke` viene attivato quando un puntatore sovrapposto che non è stato creato nell'heap di Garbage Collection viene passato a funzioni Win32 specifiche.  
  
> [!NOTE]
>  Per impostazione predefinita, questo assistente al debug gestito viene attivato solo se la chiamata di pInvoke è definita nel codice e tramite il debugger viene segnalato lo stato JustMyCode di ciascun metodo. Un debugger che non riconosce JustMyCode, ad esempio MDbg.exe senza estensioni, non attiva questo assistente al debug gestito. L'assistente al debug gestito può essere abilitato per questi debugger usando un file di configurazione e impostando in modo esplicito `justMyCode="false"` nel file con estensione mda.config `(<invalidOverlappedToPinvoke enable="true" justMyCode="false"/>`).  
  
## <a name="symptoms"></a>Sintomi  
 Arresto anomalo o danneggiamento inspiegabile dell'heap.  
  
## <a name="cause"></a>Causa  
 Un puntatore sovrapposto che non è stato creato nell'heap di Garbage Collection viene passato a funzioni del sistema operativo specifiche.  
  
 La tabella seguente mostra le funzioni monitorate dall'assistente al debug gestito.  
  
|Modulo|Funzione|  
|------------|--------------|  
|HttpApi.dll|`HttpReceiveHttpRequest`|  
|IpHlpApi.dll|`NotifyAddrChange`|  
|kernel32.dll|`ReadFile`|  
|kernel32.dll|`ReadFileEx`|  
|kernel32.dll|`WriteFile`|  
|kernel32.dll|`WriteFileEx`|  
|kernel32.dll|`ReadDirectoryChangesW`|  
|kernel32.dll|`PostQueuedCompletionStatus`|  
|MSWSock.dll|`ConnectEx`|  
|WS2_32.dll|`WSASend`|  
|WS2_32.dll|`WSASendTo`|  
|WS2_32.dll|`WSARecv`|  
|WS2_32.dll|`WSARecvFrom`|  
|MQRT.dll|`MQReceiveMessage`|  
  
 Il rischio di danneggiamento dell'heap è elevato in questa condizione, perché l'oggetto <xref:System.AppDomain> che effettua la chiamata può essere scaricato. Se <xref:System.AppDomain> viene scaricato, il codice dell'applicazione libera la memoria per il puntatore sovrapposto, causando il danneggiamento al termine dell'operazione, oppure il codice perde la memoria, provocando difficoltà in seguito.  
  
## <a name="resolution"></a>Risoluzione  
 Usare un oggetto <xref:System.Threading.Overlapped>, chiamando il metodo <xref:System.Threading.Overlapped.Pack%2A> per ottenere una struttura <xref:System.Threading.NativeOverlapped> che possa essere passata alla funzione. Se <xref:System.AppDomain> viene scaricato, CLR attende il completamento dell'operazione asincrona prima di liberare il puntatore.  
  
## <a name="effect-on-the-runtime"></a>Effetto sull'ambiente di esecuzione  
 L'assistente al debug gestito non ha alcun effetto su CLR.  
  
## <a name="output"></a>Output  
 Di seguito è riportato un esempio di output di questo assistente al debug gestito.  
  
 `An overlapped pointer (0x00ea3430) that was not allocated on the GC heap was passed via Pinvoke to the Win32 function 'WriteFile' in module 'KERNEL32.DLL'. If the AppDomain is shut down, this can cause heap corruption when the async I/O completes. The best solution is to pass a NativeOverlapped structure retrieved from a call to System.Threading.Overlapped.Pack(). If the AppDomain exits, the CLR will keep this structure alive and pinned until the I/O completes.`  
  
## <a name="configuration"></a>Configurazione  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidOverlappedToPinvoke/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Diagnostica degli errori tramite gli assistenti al debug gestito](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
- [Marshalling di interoperabilità](../../../docs/framework/interop/interop-marshaling.md)
