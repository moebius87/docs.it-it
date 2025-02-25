---
title: MDA invalidGCHandleCookie
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), invalid cookies
- cookies, invalid
- managed debugging assistants (MDAs), invalid cookies
- InvalidGCHandleCookie MDA
- invalid cookies
ms.assetid: 613ad742-3c11-401d-a6b3-893ceb8de4f8
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 876f0fe3c40cb6754b4ba714833dd160dc4de3a8
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61754401"
---
# <a name="invalidgchandlecookie-mda"></a>MDA invalidGCHandleCookie
L'assistente al debug gestito `invalidGCHandleCookie` viene attivato quando si tenta una conversione da un cookie <xref:System.IntPtr> non valido a un oggetto <xref:System.Runtime.InteropServices.GCHandle>.  
  
## <a name="symptoms"></a>Sintomi  
 Comportamento indefinito, ad esempio violazioni di accesso e danneggiamento della memoria durante il tentativo di usare o recuperare un oggetto <xref:System.Runtime.InteropServices.GCHandle> da un cookie <xref:System.IntPtr>.  
  
## <a name="cause"></a>Causa  
 Il cookie è probabilmente non valido perché in origine non è stato creato da un oggetto <xref:System.Runtime.InteropServices.GCHandle>, rappresenta un oggetto <xref:System.Runtime.InteropServices.GCHandle> che è già stato liberato, è un cookie per un oggetto <xref:System.Runtime.InteropServices.GCHandle> in un dominio dell'applicazione diverso o è stato sottoposto a marshalling al codice nativo come <xref:System.Runtime.InteropServices.GCHandle> ma passato di nuovo a CLR come oggetto <xref:System.IntPtr>, in cui è stato tentato un cast.  
  
## <a name="resolution"></a>Risoluzione  
 Specificare un cookie <xref:System.IntPtr> valido per <xref:System.Runtime.InteropServices.GCHandle>.  
  
## <a name="effect-on-the-runtime"></a>Effetto sull'ambiente di esecuzione  
 Quando questo assistente al debug gestito è abilitato, il debugger non è più in grado di tracciare di nuovo le radici ai rispettivi oggetti perché i valori del cookie restituiti sono diversi da quelli restituiti quando l'assistente al debug gestito non è abilitato.  
  
## <a name="output"></a>Output  
 Viene segnalato il valore non valido del cookie <xref:System.IntPtr>.  
  
## <a name="configuration"></a>Configurazione  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidGCHandleCookie />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Runtime.InteropServices.GCHandle.FromIntPtr%2A>
- <xref:System.Runtime.InteropServices.GCHandle>
- [Diagnostica degli errori tramite gli assistenti al debug gestito](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
