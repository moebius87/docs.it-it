---
title: Creazione di un oggetto DataTable da una query (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1b97afeb-03f8-41e2-8eb3-58aff65f7d18
ms.openlocfilehash: b25de14267bc31ad0ac5e3f51d4cd964b5a0535f
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61607336"
---
# <a name="creating-a-datatable-from-a-query-linq-to-dataset"></a>Creazione di un oggetto DataTable da una query (LINQ to DataSet)
L'associazione dati è un utilizzo comune dell'oggetto <xref:System.Data.DataTable>. Il metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> copia i dati dei risultati di una query in un oggetto <xref:System.Data.DataTable> che può essere quindi usato per il data binding. Dopo l'esecuzione delle operazioni sui dati, il nuovo oggetto <xref:System.Data.DataTable> viene nuovamente unito nell'oggetto <xref:System.Data.DataTable> di origine.  
  
 Il metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> usa il processo seguente per creare <xref:System.Data.DataTable> da una query.  
  
1. Il metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> duplica un oggetto <xref:System.Data.DataTable> della tabella di origine, ovvero un oggetto <xref:System.Data.DataTable> che implementa l'interfaccia <xref:System.Linq.IQueryable%601>. L'oggetto <xref:System.Collections.IEnumerable> di origine viene solitamente generato da una query di espressione o metodo [!INCLUDE[linq_dataset](../../../../includes/linq-dataset-md.md)].  
  
2. Lo schema dell'oggetto <xref:System.Data.DataTable> duplicato viene compilato dalle colonne del primo oggetto <xref:System.Data.DataRow> enumerato nella tabella di origine, mentre il nome della tabella duplicata corrisponde al nome della tabella di origine con l'aggiunta del termine "query" alla fine.  
  
3. Il contenuto di ogni riga della tabella di origine viene copiato in un nuovo oggetto <xref:System.Data.DataRow>, che viene quindi inserito nella tabella duplicata. Le proprietà <xref:System.Data.DataRow.RowState%2A> e <xref:System.Data.DataRow.RowError%2A> vengono mantenuta durante l'operazione di copia. Se gli oggetti <xref:System.ArgumentException> nell'origine provengono da tabelle diverse, viene generata un'eccezione <xref:System.Data.DataRow>.  
  
4. L'oggetto <xref:System.Data.DataTable> duplicato viene restituito dopo che sono stati copiati tutti gli oggetti <xref:System.Data.DataRow> presenti nella tabella di input sottoposta a query. Se la sequenza di origine non contiene oggetti <xref:System.Data.DataRow>, il metodo restituisce un oggetto <xref:System.Data.DataTable> vuoto.  
  
 Si noti che la chiamata al metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> genererà l'esecuzione della query associata alla tabella di origine.  
  
 Quando il metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> rileva un riferimento Null o un tipo di valore nullable in una riga della tabella di origine, sostituisce il valore con <xref:System.DBNull.Value>. I valori Null vengono gestiti correttamente nell'oggetto <xref:System.Data.DataTable> restituito.  
  
 Nota: Il <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> metodo accetta come input una query che può restituire righe da più <xref:System.Data.DataTable> o <xref:System.Data.DataSet> oggetti. Il metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> consente di copiare i dati ma non le proprietà dagli oggetti <xref:System.Data.DataTable> o <xref:System.Data.DataSet> di origine nell'oggetto <xref:System.Data.DataTable> restituito. È necessario impostare in modo esplicito le proprietà nell'oggetto <xref:System.Data.DataTable> restituito, ad esempio <xref:System.Data.DataTable.Locale%2A> e <xref:System.Data.DataTable.TableName%2A>.  
  
 Nell'esempio seguente viene eseguita una query sulla tabella SalesOrderHeader per individuare gli ordini effettuati dopo l'8 agosto 2001 e viene usato il metodo <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> per creare un oggetto <xref:System.Data.DataTable> dalla query. <xref:System.Data.DataTable> viene quindi associato a <xref:System.Windows.Forms.BindingSource>, che funge da proxy per <xref:System.Windows.Forms.DataGridView>.  
  
 [!code-csharp[DP LINQ to DataSet Examples#CopyToDataTable1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#copytodatatable1)]
 [!code-vb[DP LINQ to DataSet Examples#CopyToDataTable1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#copytodatatable1)]  
  
## <a name="creating-a-custom-copytodatatablet-method"></a>Creazione di un CopyToDataTable personalizzato\<T > (metodo)  
 I metodi <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> esistenti funzionano solo su un'origine <xref:System.Collections.Generic.IEnumerable%601> in cui il parametro generico `T` è di tipo <xref:System.Data.DataRow>. Sebbene sia utile, questa funzionalità non consente di creare tabelle da una sequenza di tipi scalari, da query che restituiscono tipi anonimi o da query che eseguono join di tabelle. Per un esempio di come implementare due custom `CopyToDataTable` metodi di caricamento di una tabella da una sequenza di tipi scalari o anonimi, vedere [come: Implementare CopyToDataTable\<T > in cui il tipo generico T non è un DataRow](../../../../docs/framework/data/adonet/implement-copytodatatable-where-type-not-a-datarow.md)s.  
  
 Per gli esempi di questa sezione vengono usati i tipi personalizzati seguenti:  
  
 [!code-csharp[DP Custom CopyToDataTable Examples#ItemClass](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#itemclass)]
 [!code-vb[DP Custom CopyToDataTable Examples#ItemClass](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#itemclass)]  
  
### <a name="example"></a>Esempio  
 In questo esempio viene eseguito un join sulle tabelle `SalesOrderHeader` e `SalesOrderDetail` per ottenere gli ordini effettuati online dal mese di agosto e viene creata una tabella dalla query.  
  
 [!code-csharp[DP Custom CopyToDataTable Examples#Join](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#join)]
 [!code-vb[DP Custom CopyToDataTable Examples#Join](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#join)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene eseguita una query su una raccolta per ottenere gli articoli con prezzo maggiore di 9,99 $ e viene creata una tabella dai risultati della query.  
  
 [!code-csharp[DP Custom CopyToDataTable Examples#LoadItemsIntoTable](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#loaditemsintotable)]
 [!code-vb[DP Custom CopyToDataTable Examples#LoadItemsIntoTable](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#loaditemsintotable)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene eseguita una query su una raccolta per ottenere gli articoli con prezzo maggiore di 9,99 $ e vengono proiettati i risultati. La sequenza di tipi anonimi restituiti viene caricata in una tabella esistente.  
  
 [!code-csharp[DP Custom CopyToDataTable Examples#LoadItemsIntoExistingTable](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#loaditemsintoexistingtable)]
 [!code-vb[DP Custom CopyToDataTable Examples#LoadItemsIntoExistingTable](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#loaditemsintoexistingtable)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene eseguita una query su una raccolta per ottenere gli articoli con prezzo maggiore di 9,99 $ e vengono proiettati i risultati. La sequenza di tipi anonimi restituiti viene caricata in una tabella esistente. Lo schema della tabella viene automaticamente espanso perché i tipi `Book` e `Movies` sono derivati dal tipo `Item`.  
  
 [!code-csharp[DP Custom CopyToDataTable Examples#LoadItemsExpandSchema](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#loaditemsexpandschema)]
 [!code-vb[DP Custom CopyToDataTable Examples#LoadItemsExpandSchema](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#loaditemsexpandschema)]  
  
### <a name="example"></a>Esempio  
 Nell'esempio seguente viene eseguita una query su una raccolta per ottenere gli articoli con prezzo maggiore di 9,99 $ e viene restituita una sequenza di oggetti <xref:System.Double>, che viene caricata in una nuova tabella.  
  
 [!code-csharp[DP Custom CopyToDataTable Examples#LoadScalarSequence](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#loadscalarsequence)]
 [!code-vb[DP Custom CopyToDataTable Examples#LoadScalarSequence](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#loadscalarsequence)]  
  
## <a name="see-also"></a>Vedere anche

- [Guida per programmatori](../../../../docs/framework/data/adonet/programming-guide-linq-to-dataset.md)
- [Metodi generici Field e SetField](../../../../docs/framework/data/adonet/generic-field-and-setfield-methods-linq-to-dataset.md)
- [Esempi di LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset-examples.md)
