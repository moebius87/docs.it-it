---
title: <CompatSortNLSVersion> Elemento
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- <CompatSortNLSVersion> element
- CompatSortNLSVersion element
ms.assetid: 782cc82e-83f7-404a-80b7-6d3061a8b6e3
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b426eaaa2dab4d54ea4c82483c079428f3bfac57
ms.sourcegitcommit: d8ebe0ee198f5d38387a80ba50f395386779334f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689917"
---
# <a name="compatsortnlsversion-element"></a>\<CompatSortNLSVersion > elemento
Specifica che nel runtime devono essere utilizzati ordinamenti legacy quando si eseguono confronti di stringhe.  
  
 \<configuration>  
\<runtime>  
\<CompatSortNLSVersion > elemento  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<CompatSortNLSVersion    
   enabled="4096"/>  
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|`enabled`|Attributo obbligatorio.<br /><br /> Specifica l'ID delle impostazioni locali di cui deve essere utilizzato l'ordinamento.|  
  
## <a name="enabled-attribute"></a>Attributo enabled  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|4096|ID impostazioni locali mediante il quale viene rappresentato un ordinamento alternativo. In questo caso, 4096 rappresenta l'ordinamento di .NET Framework 3.5 e versioni precedenti.|  
  
### <a name="child-elements"></a>Elementi figlio  
 Nessuno.  
  
### <a name="parent-elements"></a>Elementi padre  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|`configuration`|Elemento radice in ciascun file di configurazione usato in Common Language Runtime e nelle applicazioni .NET Framework.|  
|`runtime`|Contiene informazioni sulle opzioni di inizializzazione in fase di esecuzione.|  
  
## <a name="remarks"></a>Note  
 Operazioni di maiuscole e minuscole, l'ordinamento e confronto di stringhe eseguita dal <xref:System.Globalization.CompareInfo?displayProperty=nameWithType> classe in .NET Framework 4 è conforme a standard Unicode 5.1, i risultati dei metodi di confronto di stringa, ad esempio <xref:System.String.Compare%28System.String%2CSystem.String%29?displayProperty=nameWithType> e <xref:System.String.LastIndexOf%28System.String%29?displayProperty=nameWithType> può differire da versioni precedenti di .NET Framework. Se l'applicazione dipende dal comportamento legacy, è possibile ripristinare il confronto di stringhe e l'ordinamento delle regole usate in .NET Framework 3.5 e versioni precedenti includendo il `<CompatSortNLSVersion>` elemento nel file di configurazione dell'applicazione.  
  
> [!IMPORTANT]
>  Per il ripristino del confronto di stringhe legacy e delle regole di ordinamento è necessaria inoltre la disponibilità della libreria di collegamento dinamico sort00001000.dll nel sistema locale.  
  
 È inoltre possibile utilizzare l'ordinamento delle stringhe legacy e le regole di confronto in un dominio dell'applicazione specifico passando la stringa "NetFx40_Legacy20SortingBehavior" al metodo <xref:System.AppDomainSetup.SetCompatibilitySwitches%2A> quando si crea il dominio dell'applicazione.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene creata un'istanza di due oggetti <xref:System.String> e viene chiamato il metodo <xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> per confrontarle utilizzando le convenzioni delle impostazioni cultura correnti.  
  
 [!code-csharp[String.BreakingChanges#1](../../../../../samples/snippets/csharp/VS_Snippets_CLR/string.breakingchanges/cs/example1.cs#1)]
 [!code-vb[String.BreakingChanges#1](../../../../../samples/snippets/visualbasic/VS_Snippets_CLR/string.breakingchanges/vb/example1.vb#1)]  
  
 Quando si esegue l'esempio in .NET Framework 4, viene visualizzato l'output seguente.  
  
```  
sta follows a in the sort order.  
```  
  
 Ciò è completamente diversa dall'output che viene visualizzato quando si esegue l'esempio in .NET Framework 3.5.  
  
```  
sta equals a in the sort order.  
```  
  
 Tuttavia, se si aggiunge il file di configurazione seguente alla directory di esempio e quindi esegue l'esempio in .NET Framework 4, l'output è identico a quello prodotto dall'esempio quando viene eseguito in .NET Framework 3.5.  
  
```xml  
<?xml version ="1.0"?>  
<configuration>  
   <runtime>  
      <CompatSortNLSVersion enabled="4096"/>  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Vedere anche

- [Schema delle impostazioni di runtime](../../../../../docs/framework/configure-apps/file-schema/runtime/index.md)
- [Schema dei file di configurazione](../../../../../docs/framework/configure-apps/file-schema/index.md)
