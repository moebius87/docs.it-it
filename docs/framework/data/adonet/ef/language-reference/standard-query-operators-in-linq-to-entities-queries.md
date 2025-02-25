---
title: Operatori di query standard in query di LINQ to Entities
ms.date: 08/21/2018
ms.assetid: 7fa55a9b-6219-473d-b1e5-2884a32dcdff
ms.openlocfilehash: 5c666bad40d0e433ee5f8d2b1155e881d7042a85
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61797722"
---
# <a name="standard-query-operators-in-linq-to-entities-queries"></a>Operatori di query standard in query di LINQ to Entities
In una query è necessario specificare le informazioni che si desidera recuperare dall'origine dati. Una query può inoltre specificare in che modo ordinare, raggruppare e formattare le informazioni prima che vengano restituite. LINQ fornisce un set di metodi di query standard che è possibile usare in una query. La maggior parte di questi metodi agisce sulle sequenze; In questo contesto, una sequenza è un oggetto il cui tipo implementa la <xref:System.Collections.Generic.IEnumerable%601> interfaccia o <xref:System.Linq.IQueryable%601> interfaccia. Le funzionalità di query degli operatori di query standard includono filtro, proiezione, aggregazione, ordinamento, raggruppamento, paging e altro ancora. Alcuni degli operatori di query standard usati più di frequente dispongono di sintassi dedicata delle parole chiave, in modo da poter essere chiamati usando la sintassi delle espressioni di query. Un'espressione di query rappresenta un modo diverso e più leggibile per esprimere una query rispetto alla sintassi equivalente basata su metodo. Le clausole di espressione di query vengono convertite in chiamate ai metodi di query in fase di compilazione. Per un elenco degli operatori query standard che hanno clausole di espressione di query equivalenti, vedere [panoramica degli operatori Query Standard](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/bb397896(v=vs.120)).  
  
 Non tutti gli operatori di query standard sono supportati nelle query [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)]. Per altre informazioni, vedere [supportati e i metodi LINQ non supportati (LINQ to Entities)](../../../../../../docs/framework/data/adonet/ef/language-reference/supported-and-unsupported-linq-methods-linq-to-entities.md). In questo argomento vengono fornite informazioni sugli operatori di query standard specifici di [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)]. Per altre informazioni sui problemi noti [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)] query, vedere [problemi noti e considerazioni in LINQ to Entities](../../../../../../docs/framework/data/adonet/ef/language-reference/known-issues-and-considerations-in-linq-to-entities.md).  
  
## <a name="projection-and-filtering-methods"></a>Metodi di proiezione e di filtro  
 *Proiezione* fa riferimento alla trasformazione degli elementi di un set di risultati in formato desiderato. È ad esempio possibile proiettare un subset delle proprietà necessarie da ciascun oggetto nel set di risultati, proiettare una proprietà ed eseguire un calcolo matematico su di essa o proiettare l'intero oggetto dal set di risultati. I metodi di proiezione sono `Select` e `SelectMany`.  
  
 *Il filtro* fa riferimento all'operazione di limitare il set di risultati a contenere solo gli elementi che corrispondono a una condizione specificata. Il metodo di filtro è `Where`.  
  
 La maggior parte degli overload dei metodi di proiezione e di filtro è supportata in [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)], fatta eccezione per gli overload che accettano un argomento posizionale.  
  
## <a name="join-methods"></a>Metodi join  
 L'unione in join è un'operazione importante nelle query destinate a origini dati che non presentano relazioni esplorabili tra loro. Per join di due origini dati si intende l'associazione degli oggetti di un'origine dati con gli oggetti dell'altra origine dati che condividono un attributo o una proprietà comune. I metodi di join sono `Join` e `GroupJoin`.  
  
 La maggior parte degli overload dei metodi di join è supportata, fatta eccezione per gli overload che usano un oggetto <xref:System.Collections.Generic.IEqualityComparer%601>, in quanto l'operatore di confronto non può essere convertito nell'origine dati.  
  
## <a name="set-methods"></a>Imposta metodi  
 Le operazioni Set in LINQ sono operazioni di query i cui set di risultati sono basati sulla presenza o sull'assenza di elementi equivalenti nella stessa raccolta o in una raccolta distinta. I metodi Set sono `All`, `Any`, `Concat`, `Contains`, `DefaultIfEmpty`, `Distinct`, `EqualAll`, `Except`, `Intersect` e `Union`.  
  
 La maggior parte degli overload dei metodi Set è supportata in [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)], sebbene vi siano alcune differenze nel comportamento rispetto a LINQ to Objects. Tuttavia, impostare i metodi che usano un <xref:System.Collections.Generic.IEqualityComparer%601> non sono supportate perché l'operatore di confronto non può essere convertito nell'origine dati.  
  
## <a name="ordering-methods"></a>Metodi di ordinamento  
 L'ordinamento consiste nell'ordinare gli elementi di un set di risultati in base a uno o più attributi. Specificando più di un criterio di ordinamento, è possibile interrompere i collegamenti all'interno di un gruppo.  
  
 La maggior parte degli overload dei metodi di ordinamento è supportata, fatta eccezione per gli overload che usano un oggetto <xref:System.Collections.Generic.IComparer%601>, in quanto l'operatore di confronto non può essere convertito nell'origine dati. I metodi di ordinamento sono `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending` e `Reverse`.  
  
 Poiché la query viene eseguita sull'origine dati, il comportamento di ordinamento può differire rispetto alle query eseguite in CLR. Questo avviene in quanto le opzioni di ordinamento, ad esempio ordinamento in base a maiuscole e minuscole, ordinamento kanji e ordinamento in base ai valori null, possono essere impostate nell'origine dati. A seconda dell'origine dati, queste opzioni di ordinamento possono produrre risultati diversi rispetto a CLR.  
  
 Se si specifica lo stesso selettore di chiave in più di un'operazione di ordinamento, viene prodotto un ordinamento duplicato. Questo comportamento non è valido e verrà pertanto generata un'eccezione.  
  
## <a name="grouping-methods"></a>Metodi di raggruppamento  
 Il raggruppamento si riferisce all'inserimento dei dati in gruppi in modo che gli elementi di ciascun gruppo condividano un attributo comune. Il metodo di raggruppamento è `GroupBy`.  
  
 La maggior parte degli overload dei metodi di raggruppamento è supportata, fatta eccezione per gli overload che usano un oggetto <xref:System.Collections.Generic.IEqualityComparer%601>, in quanto l'operatore di confronto non può essere convertito nell'origine dati.  
  
 I metodi di raggruppamento vengono mappati all'origine usando una sottoquery distinta per il selettore di chiave. La sottoquery di confronto del selettore di chiave viene eseguita usando la semantica dell'origine dati, includendo i problemi relativi al confronto di valori `null`.  
  
## <a name="aggregate-methods"></a>Metodi di aggregazione  
 Un'operazione di aggregazione calcola un singolo valore da una raccolta di valori. Il calcolo della temperatura media giornaliera dai valori della temperatura giornaliera di un mese rappresenta ad esempio un'operazione di aggregazione. I metodi di aggregazione sono `Aggregate`, `Average`, `Count`, `LongCount`, `Max`, `Min` e `Sum`.  
  
 La maggior parte degli overload dei metodi di aggregazione è supportata. Per il comportamento relativo ai valori null, i metodi di aggregazione usano la semantica dell'origine dati. Il comportamento dei metodi di aggregazione quando sono coinvolti valori null potrebbe essere diverso, a seconda dell'origine dati back-end usata. Il comportamento dei metodi di aggregazione quando viene usata la semantica dell'origine dati potrebbe inoltre essere diverso da quello previsto per i metodi CLR. Il comportamento predefinito per il metodo `Sum` in SQL Server consiste ad esempio nell'ignorare qualsiasi valore null anziché generare un'eccezione.  
  
 Le eccezioni dovute all'aggregazione, ad esempio un overflow causato dalla funzione `Sum`, vengono generate come eccezioni dell'origine dati o eccezioni di Entity Framework durante la materializzazione dei risultati di query.  
  
 Per i metodi che comportano un calcolo in una sequenza, ad esempio `Sum` o `Average`, il calcolo effettivo viene eseguito nel server. Di conseguenza, nel server potrebbero verificarsi conversioni di tipi e perdita di precisione e i risultati potrebbero differire da quelli previsti in caso di utilizzo della semantica CLR.  
  
 Il comportamento predefinito dei metodi di aggregazione per i valori null/non null è illustrato nella tabella seguente:  
  
|Metodo|Nessun dato|Tutti valori null|Alcuni valori null|Nessun valore null|  
|------------|-------------|---------------------|----------------------|--------------------|  
|`Average`|Restituisce null.|Restituisce null.|Restituisce la media dei valori non null in una sequenza.|Calcola la media di una sequenza di valori numerici.|  
|`Count`|Restituisce 0.|Restituisce il numero di valori null nella sequenza.|Restituisce il numero di valori null e non null nella sequenza.|Restituisce il numero di elementi nella sequenza.|  
|`Max`|Restituisce null.|Restituisce null.|Restituisce il valore massimo non null in una sequenza.|Restituisce il valore massimo in una sequenza.|  
|`Min`|Restituisce null.|Restituisce null.|Restituisce il valore minimo non null in una sequenza.|Restituisce il valore minimo in una sequenza.|  
|`Sum`|Restituisce null.|Restituisce null.|Restituisce la somma dei valori non null in una sequenza.|Calcola la somma di una sequenza di valori numerici.|  
  
## <a name="type-methods"></a>Metodi di tipo  
 I due metodi LINQ che gestiscono con conversione del tipo e il test sono entrambi supportati nel contesto di Entity Framework. Ciò significa che gli unici tipi supportati sono i tipi che eseguono il mapping al tipo appropriato di Entity Framework. Per un elenco di questi tipi, vedere [tipi di modello concettuale (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#conceptual-model-types-csdl). I metodi di tipo sono `Convert` e `OfType`.  
  
 `OfType` è supportato per i tipi di entità. `Convert` è supportato per i tipi primitivi del modello concettuale.  Sono supportati anche i metodi `is` e `as` C#.  
  
## <a name="paging-methods"></a>Metodi di paging  
 Operazioni di paging restituiscono un singolo elemento o più elementi di una sequenza. I metodi di paging supportati sono `First`, `FirstOrDefault`, `Single`, `SingleOrDefault`, `Skip`, e `Take`.  
  
 Un numero di metodi di paging non è supportato, a causa dell'impossibilità di eseguire il mapping di funzioni all'origine dati o la mancanza di ordinamento implicito dei set nell'origine dati. I metodi che restituiscono un valore predefinito sono limitati ai tipi primitivi del modello concettuale e ai tipi di riferimento con valori predefiniti null. I metodi di paging eseguiti in una sequenza vuota restituiscono null.  
  
## <a name="see-also"></a>Vedere anche

- [Metodi LINQ supportati e non supportati (LINQ to Entities)](../../../../../../docs/framework/data/adonet/ef/language-reference/supported-and-unsupported-linq-methods-linq-to-entities.md)
- [Cenni preliminari sugli operatori di query standard](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/bb397896(v=vs.120))
