---
title: Filtro con DataView (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5632d74a-ff53-4ea7-9fe7-4a148eeb1c68
ms.openlocfilehash: b41b95ba06f031dc45c0267432d0d6afb7f3a7d9
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64645694"
---
# <a name="filtering-with-dataview-linq-to-dataset"></a>Filtro con DataView (LINQ to DataSet)
La possibilità di filtrare i dati in base a criteri specifici e quindi di presentarli a un client tramite un controllo dell'interfaccia utente rappresenta un aspetto importante dell'associazione dati. Con <xref:System.Data.DataView> è possibile filtrare i dati e restituire subset di righe di dati che soddisfano criteri specifici di filtro. Oltre a basato su stringa filtraggio <xref:System.Data.DataView> inoltre offre la possibilità di usare [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] espressioni per i criteri di filtro. [!INCLUDE[vbteclinq](../../../../includes/vbteclinq-md.md)] le espressioni consentono operazioni di filtro più complesse e potenti rispetto al filtro basato su stringa.  
  
 Per filtrare i dati tramite <xref:System.Data.DataView>, sono disponibili due modalità:  
  
- Creare <xref:System.Data.DataView> da una query [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)] con una clausola Where.  
  
- Usare le funzionalità di filtro basate su stringa esistenti di <xref:System.Data.DataView>.  
  
## <a name="creating-dataview-from-a-query-with-filtering-information"></a>Creazione di DataView da una query con informazioni di filtro  
 È possibile creare un oggetto <xref:System.Data.DataView> da una query [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)]. Se la query contiene una clausola `Where`, l'oggetto <xref:System.Data.DataView> viene creato con le informazioni di filtro della query. L'espressione nella clausola `Where` viene usata per determinare le righe di dati da includere in <xref:System.Data.DataView> e rappresenta la base per il filtro.  
  
 I filtri basati su espressione sono più potenti e complessi rispetto a quelli più semplici basati su stringa. I filtri basati su stringa e quelli basati su espressione si escludono a vicenda. Se si imposta un oggetto <xref:System.Data.DataView.RowFilter%2A> basato su stringa dopo la creazione di un oggetto <xref:System.Data.DataView> da una query, il filtro basato su espressione inferito dalla query viene cancellato.  
  
> [!NOTE]
>  Nella maggior parte dei casi le espressioni usate per il filtro non devono presentare effetti collaterali e devono essere deterministiche. Le espressioni non devono inoltre contenere eventuale codice che dipende da un numero impostato di esecuzioni perché è possibile che le operazioni di filtro vengano eseguite un numero qualsiasi di volte.  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene eseguita una query sulla tabella SalesOrderDetail per individuare gli ordini con una quantità maggiore di 2 e minore di 6. Viene quindi creato un oggetto <xref:System.Data.DataView> dalla query e l'oggetto <xref:System.Data.DataView> viene infine associato a <xref:System.Windows.Forms.BindingSource>.  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhere](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywhere)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhere](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywhere)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene creato un oggetto <xref:System.Data.DataView> da una query eseguita per individuare gli ordini effettuati dopo il 6 giugno 2001:  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhere3](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywhere3)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhere3](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywhere3)]  
  
### <a name="example"></a>Esempio  
 Il filtro può essere anche combinato anche con l'ordinamento. Nell'esempio seguente viene creato un oggetto <xref:System.Data.DataView> da una query eseguita per individuare i contatti il cui cognome inizia con "S", quindi i risultati vengono ordinati in base a cognome e poi nome:  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhereOrderByThenBy](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywhereorderbythenby)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhereOrderByThenBy](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywhereorderbythenby)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene usato l'algoritmo SoundEx per individuare i contatti il cui cognome è simile a "Zhu". L'algoritmo SoundEx è implementato nel metodo SoundEx.  
  
 [!code-csharp[DP DataView Samples#LDVSoundExFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvsoundexfilter)]
 [!code-vb[DP DataView Samples#LDVSoundExFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvsoundexfilter)]  
  
 SoundEx è un algoritmo fonetico, originariamente sviluppato dal Census Bureau USA, che viene usato per indicizzare i nomi in base al suono, così come vengono pronunciati in inglese. Census Bureau. Il metodo SoundEx restituisce un codice a quattro caratteri per un nome, costituito da una lettera seguita da tre numeri. La lettera corrisponde all'iniziale, mentre i numeri codificano le altre consonanti del nome. I nomi  pronunciati in modo simile condividono lo stesso codice SoundEx. Di seguito è illustrata l'implementazione SoundEx usata nel metodo SoundEx dell'esempio precedente:  
  
 [!code-csharp[DP DataView Samples#SoundEx](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#soundex)]
 [!code-vb[DP DataView Samples#SoundEx](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#soundex)]  
  
## <a name="using-the-rowfilter-property"></a>Utilizzo della proprietà RowFilter  
 La funzionalità di filtro basata su stringa esistente di <xref:System.Data.DataView> è comunque utilizzabile nel contesto [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)]. Per altre informazioni sulla stringa basata <xref:System.Data.DataView.RowFilter%2A> applicazione di filtri, vedere [ordinamento e filtro dei dati](../../../../docs/framework/data/adonet/dataset-datatable-dataview/sorting-and-filtering-data.md).  
  
 Nell'esempio seguente viene creato un oggetto <xref:System.Data.DataView> dalla tabella Contact e viene quindi impostata la proprietà <xref:System.Data.DataView.RowFilter%2A> per restituire le righe in cui il cognome del contatto è "Zhu".  
  
 [!code-csharp[DP DataView Samples#LDVRowFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvrowfilter)]
 [!code-vb[DP DataView Samples#LDVRowFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvrowfilter)]  
  
 Dopo la creazione di un oggetto <xref:System.Data.DataView> da un oggetto <xref:System.Data.DataTable> o da una query [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)], è possibile usare la proprietà <xref:System.Data.DataView.RowFilter%2A> per specificare subset di righe basati sui relativi valori di colonna. I filtri basati su stringa e quelli basati su espressione si escludono a vicenda. Impostando il <xref:System.Data.DataView.RowFilter%2A> proprietà verrà cancellata l'espressione di filtro dedotto dal [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)] query e l'espressione di filtro non può essere reimpostata.  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhereSetRowFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywheresetrowfilter)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhereSetRowFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywheresetrowfilter)]  
  
 Per restituire i risultati di una particolare query sui dati anziché fornire una visualizzazione dinamica di un subset di dati, è possibile usare i metodi <xref:System.Data.DataView.Find%2A> o <xref:System.Data.DataView.FindRows%2A> di <xref:System.Data.DataView> anziché impostare la proprietà <xref:System.Data.DataView.RowFilter%2A>. L'utilizzo della proprietà <xref:System.Data.DataView.RowFilter%2A> è consigliato in applicazioni con associazioni a dati, in cui i risultati filtrati vengono visualizzati da un controllo associato. L'impostazione della proprietà <xref:System.Data.DataView.RowFilter%2A> provoca una ricompilazione dell'indice dei dati, aggiungendo un sovraccarico all'applicazione e riducendo le prestazioni. I metodi <xref:System.Data.DataView.Find%2A> e <xref:System.Data.DataView.FindRows%2A> si avvalgono dell'indice corrente, senza che sia necessario ricompilarlo. Se si intende chiamare <xref:System.Data.DataView.Find%2A> o <xref:System.Data.DataView.FindRows%2A> solo una volta, è opportuno usare l'oggetto <xref:System.Data.DataView> esistente. Se invece si intende chiamare <xref:System.Data.DataView.Find%2A> o <xref:System.Data.DataView.FindRows%2A> più volte, è opportuno creare un nuovo oggetto <xref:System.Data.DataView> per ricompilare l'indice sulla colonna da usare per la ricerca e quindi chiamare il metodo <xref:System.Data.DataView.Find%2A> o <xref:System.Data.DataView.FindRows%2A>. Per altre informazioni sul <xref:System.Data.DataView.Find%2A> e <xref:System.Data.DataView.FindRows%2A> vedere metodi [ricerca di righe](../../../../docs/framework/data/adonet/dataset-datatable-dataview/finding-rows.md) e [prestazioni di DataView](../../../../docs/framework/data/adonet/dataview-performance.md).  
  
## <a name="clearing-the-filter"></a>Cancellazione del filtro  
 È possibile cancellare il filtro su un oggetto <xref:System.Data.DataView>, impostato tramite la proprietà <xref:System.Data.DataView.RowFilter%2A>. Per cancellare un filtro su <xref:System.Data.DataView>, sono disponibili due modalità:  
  
- Impostare la proprietà <xref:System.Data.DataView.RowFilter%2A> su `null`.  
  
- Impostare la proprietà <xref:System.Data.DataView.RowFilter%2A> su una stringa vuota.  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene creato un oggetto <xref:System.Data.DataView> da una query, quindi il filtro viene cancellato impostando la proprietà <xref:System.Data.DataView.RowFilter%2A> su `null`:  
  
 [!code-csharp[DP DataView Samples#LDVClearRowFilter2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvclearrowfilter2)]
 [!code-vb[DP DataView Samples#LDVClearRowFilter2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvclearrowfilter2)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene creato un oggetto <xref:System.Data.DataView> da una tabella e viene impostata la proprietà <xref:System.Data.DataView.RowFilter%2A>. Il filtro viene quindi cancellato impostando la proprietà <xref:System.Data.DataView.RowFilter%2A> su una stringa vuota:  
  
 [!code-csharp[DP DataView Samples#LDVClearRowFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvclearrowfilter)]
 [!code-vb[DP DataView Samples#LDVClearRowFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvclearrowfilter)]  
  
## <a name="see-also"></a>Vedere anche

- [Data binding e LINQ to DataSet](../../../../docs/framework/data/adonet/data-binding-and-linq-to-dataset.md)
- [Ordinamento con DataView](../../../../docs/framework/data/adonet/sorting-with-dataview-linq-to-dataset.md)
