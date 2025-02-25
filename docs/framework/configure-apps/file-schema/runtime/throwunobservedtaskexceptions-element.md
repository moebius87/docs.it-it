---
title: <ThrowUnobservedTaskExceptions> Elemento
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ThrowUnobservedTaskExceptions element
- <ThrowUnobservedTaskExceptions> element
ms.assetid: cea7e588-8b8d-48d2-9ad5-8feaf3642c18
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 9647297bf976d26a97be0da8807d607789e8a065
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66489581"
---
# <a name="throwunobservedtaskexceptions-element"></a>\<ThrowUnobservedTaskExceptions > elemento
Specifica se le eccezioni di attività non gestite devono comportare l'arresto di un processo in esecuzione.  
  
 \<configuration>  
\<runtime>  
\<ThrowUnobservedTaskExceptions>  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<ThrowUnobservedTaskExceptions  
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|`enabled`|Attributo obbligatorio.<br /><br /> Specifica se le eccezioni di attività non gestite devono comportare l'arresto del processo in esecuzione.|  
  
## <a name="enabled-attribute"></a>Attributo enabled  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|`false`|Non termina il processo in esecuzione per un'eccezione di attività non gestita. Questa è l'impostazione predefinita.|  
|`true`|Termina il processo in esecuzione per un'eccezione di attività non gestita.|  
  
### <a name="child-elements"></a>Elementi figlio  
 Nessuno.  
  
### <a name="parent-elements"></a>Elementi padre  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|`configuration`|Elemento radice in ciascun file di configurazione usato in Common Language Runtime e nelle applicazioni .NET Framework.|  
|`runtime`|Contiene informazioni sulle opzioni di inizializzazione in fase di esecuzione.|  
|||  
  
## <a name="remarks"></a>Note  
 Se un'eccezione associata a un oggetto <xref:System.Threading.Tasks.Task> non è stata osservata, non esiste alcuna operazione <xref:System.Threading.Tasks.Task.Wait%2A>, l'elemento padre non è associato, la proprietà <xref:System.Threading.Tasks.Task.Exception%2A?displayProperty=nameWithType> non è stata letta e l'eccezione di attività viene considerata non osservata.  
  
 In .NET Framework 4, per impostazione predefinita, se un <xref:System.Threading.Tasks.Task> che dispone di un non osservata eccezione è sottoposto a garbage collection, il finalizzatore viene generata un'eccezione e termina il processo. L'interruzione del processo è determinata dall'intervallo di Garbage Collection e finalizzazione.  
  
 Per renderne più semplice per gli sviluppatori di scrivere codice asincrono basato su attività, .NET Framework 4.5 modifica questo comportamento predefinito per le eccezioni non osservate. Le eccezioni non osservate comportano ancora la generazione dell'evento <xref:System.Threading.Tasks.TaskScheduler.UnobservedTaskException>, ma, per impostazione predefinita, il processo non viene terminato. Invece, l'eccezione viene ignorata dopo la generazione dell'evento, indipendentemente dal fatto che l'eccezione venga rilevata da un gestore eventi.  
  
 In .NET Framework 4.5, è possibile usare la [ \<ThrowUnobservedTaskExceptions > elemento](../../../../../docs/framework/configure-apps/file-schema/runtime/throwunobservedtaskexceptions-element.md) in un file di configurazione dell'applicazione per abilitare il comportamento di .NET Framework 4 di generare un'eccezione.  
  
 Il comportamento dell'eccezione può anche essere specificato in uno dei modi seguenti:  
  
- Impostando la variabile di ambiente `COMPlus_ThrowUnobservedTaskExceptions` (`set COMPlus_ThrowUnobservedTaskExceptions=1`).  
  
- Impostando il valore DWORD del Registro di sistema valore ThrowUnobservedTaskExceptions = 1 in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\. Chiave NETFramework.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene illustrato come abilitare la generazione di eccezioni nelle attività tramite un file di configurazione dell'applicazione.  
  
```xml  
<configuration>   
    <runtime>   
        <ThrowUnobservedTaskExceptions enabled="true"/>   
    </runtime>   
</configuration>  
```  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene illustrata la modalità di generazione di un'eccezione non osservata da parte di un'attività. Il codice deve essere eseguito come programma rilasciato per funzionare correttamente.  
  
 [!code-csharp[ThrowUnobservedTaskExceptions#1](../../../../../samples/snippets/csharp/VS_Snippets_CLR/throwunobservedtaskexceptions/cs/program.cs#1)]
 [!code-vb[ThrowUnobservedTaskExceptions#1](../../../../../samples/snippets/visualbasic/VS_Snippets_CLR/throwunobservedtaskexceptions/vb/program.vb#1)]  
  
## <a name="see-also"></a>Vedere anche

- [Schema delle impostazioni di runtime](../../../../../docs/framework/configure-apps/file-schema/runtime/index.md)
- [Schema dei file di configurazione](../../../../../docs/framework/configure-apps/file-schema/index.md)
