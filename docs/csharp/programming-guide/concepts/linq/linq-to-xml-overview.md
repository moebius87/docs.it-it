---
title: Panoramica di LINQ to XML (C#)
ms.date: 10/30/2018
ms.assetid: 716b94d3-0091-4de1-8e05-41bc069fa9dd
ms.openlocfilehash: 2394151d855d75b418616d9fa1a6f69b08cdef86
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65585753"
---
# <a name="linq-to-xml-overview-c"></a>Panoramica di LINQ to XML (C#)

XML è stato ampiamente adottato per la formattazione dei dati in una vasta gamma di contesti. Viene ad esempio usato in applicazioni Web, file di configurazione, file di Microsoft Office Word e in database.

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] costituisce un approccio aggiornato e ridisegnato alla programmazione con XML. Fornisce funzionalità di modifica dei documenti in memoria di Document Object Model (DOM) e supporta espressioni di query [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. Sebbene sintatticamente diverse da XPath, queste espressioni di query offrono funzionalità simili.

## <a name="linq-to-xml-developers"></a>Sviluppatori LINQ to XML

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è destinato a diversi tipi di sviluppatori. Per un sviluppatore medio che desidera solo poter eseguire una determinata operazione, [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] semplifica il codice XML e consente di eseguire query in modo simile a SQL. Sono poi sufficienti competenze minime per consentire a un programmatore di imparare a scrivere query potenti e meno estese nel linguaggio di programmazione desiderato.

Gli sviluppatori professionisti possono usare [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] per incrementare notevolmente la propria produttività. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] consente di scrivere una minor quantità di codice, che tuttavia risulta più espressivo, compatto e potente. È inoltre possibile usare espressioni di query da più domini di dati contemporaneamente.

## <a name="what-is-linq-to-xml"></a>Informazioni su LINQ to XML

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è un'interfaccia di programmazione XML in memoria con supporto LINQ che consente di usare codice XML dall'interno dei linguaggi di programmazione .NET Framework.

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è simile al modello DOM (Document Object Model) in quanto porta in memoria il documento XML. È quindi possibile eseguire query e modificare il documento e dopo averlo modificato salvarlo in un file o serializzarlo e inviarlo tramite Internet. Tuttavia [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è diverso da DOM: offre un nuovo modello a oggetti più leggero e facile da usare che sfrutta le funzionalità del linguaggio in C#.

Il principale vantaggio di [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è costituito dall'integrazione con [!INCLUDE[vbteclinqext](~/includes/vbteclinqext-md.md)]. Grazie a tale integrazione è possibile scrivere query sul documento XML in memoria per recuperare raccolte di elementi e di attributi. La funzionalità di query di [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è paragonabile a XPath e XQuery, dal punto di vista funzionale ma non sintattico. L'integrazione di [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] in C# offre una tipizzazione più forte, il controllo in fase di compilazione e il supporto migliorato del debugger.

Un altre vantaggio di [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], costituito dalla possibilità di usare risultati di query come parametri di costruttori di oggetti <xref:System.Xml.Linq.XElement> e <xref:System.Xml.Linq.XAttribute>, consente di disporre di un approccio potente per la creazione di alberi XML. Questo approccio, chiamato *costruzione funzionale* consente agli sviluppatori di trasformare facilmente gli alberi XML da una forma all'altra.

Si supponga ad esempio di avere un tipico ordine di acquisto in formato XML come descritto in [File XML di esempio: tipico ordine di acquisto (LINQ to XML)](sample-xml-file-typical-purchase-order-linq-to-xml-1.md). Usando [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], è possibile eseguire la query seguente per ottenere il valore dell'attributo relativo al numero di parte di ciascun articolo incluso dell'ordine di acquisto:

```csharp
// Load the XML file from our project directory containing the purchase orders
var filename = "PurchaseOrder.xml";
var currentDirectory = Directory.GetCurrentDirectory();
var purchaseOrderFilepath = Path.Combine(currentDirectory, filename);

XElement purchaseOrder = XElement.Load($"{purchaseOrderFilepath}");

IEnumerable<string> partNos =  from item in purchaseOrder.Descendants("Item")
                               select (string) item.Attribute("PartNumber");
```

Ciò può essere riscritto nel modulo di sintassi del metodo:

```csharp
IEnumerable<string> partNos = purchaseOrder.Descendants("Item").Select(x => (string) x.Attribute("PartNumber"));
```

Si supponga ancora, ad esempio, di voler creare un elenco, ordinato in base al numero di parte, degli articoli il cui valore è maggiore di 100 dollari. Per ottenere queste informazioni, è possibile eseguire la query seguente:

```csharp
// Load the XML file from our project directory containing the purchase orders
var filename = "PurchaseOrder.xml";
var currentDirectory = Directory.GetCurrentDirectory();
var purchaseOrderFilepath = Path.Combine(currentDirectory, filename);

XElement purchaseOrder = XElement.Load($"{purchaseOrderFilepath}");

IEnumerable<XElement> pricesByPartNos =  from item in purchaseOrder.Descendants("Item")
                                 where (int) item.Element("Quantity") * (decimal) item.Element("USPrice") > 100
                                 orderby (string)item.Element("PartNumber")
                                 select item;
```

Ciò può essere riscritto nel modulo di sintassi del metodo:

```csharp
IEnumerable<XElement> pricesByPartNos = purchaseOrder.Descendants("Item")
                                        .Where(item => (int)item.Element("Quantity") * (decimal)item.Element("USPrice") > 100)
                                        .OrderBy(order => order.Element("PartNumber"));
```

Oltre alle funzionalità [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] include un'interfaccia di programmazione XML migliorata. Usando [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è possibile:

- Caricare XML da [file](how-to-load-xml-from-a-file.md) o [flussi](how-to-stream-xml-fragments-from-an-xmlreader.md).

- Serializzare codice XML in file o flussi.

- Creare codice XML nuovo usando la costruzione funzionale.

- Eseguire una query su codice XML usando assi simili a XPath.

- Modificare l'albero XML in memoria usando metodi quali esempio <xref:System.Xml.Linq.XContainer.Add%2A>, <xref:System.Xml.Linq.XNode.Remove%2A>, <xref:System.Xml.Linq.XNode.ReplaceWith%2A>e <xref:System.Xml.Linq.XElement.SetValue%2A>.

- Convalidare alberi XML usando lo schema XSD.

- Usare una combinazione di queste funzionalità per trasformare strutture ad albero XML da una forma in un altra.

## <a name="creating-xml-trees"></a>Creazione di strutture ad albero XML

Uno dei vantaggi più significativi della programmazione con [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è rappresentato dalla facilità con cui è possibile creare alberi XML. Ad esempio, per creare un piccolo albero XML, è possibile scrivere il codice seguente:

```csharp
XElement contacts =
new XElement("Contacts",
    new XElement("Contact",
        new XElement("Name", "Patrick Hines"),
        new XElement("Phone", "206-555-0144",
            new XAttribute("Type", "Home")),
        new XElement("phone", "425-555-0145",
            new XAttribute("Type", "Work")),
        new XElement("Address",
            new XElement("Street1", "123 Main St"),
            new XElement("City", "Mercer Island"),
            new XElement("State", "WA"),
            new XElement("Postal", "68042")
        )
    )
);
```

Per altre informazioni, vedere [Creazione di alberi XML (C#)](../../../../csharp/programming-guide/concepts/linq/creating-xml-trees.md).

## <a name="see-also"></a>Vedere anche

- <xref:System.Xml.Linq>
- [Introduzione (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/getting-started-linq-to-xml.md)
