---
title: Trasformazione funzionale di XML (C#)
ms.date: 07/20/2015
ms.assetid: 0ccb9251-38d7-44e3-9b84-1b5fe25e4b59
ms.openlocfilehash: b1325644873db29b2c40901ded3eb254b3a31073
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66485966"
---
# <a name="functional-transformation-of-xml-c"></a>Trasformazione funzionale di XML (C#)
In questo argomento viene illustrato l'approccio alla modifica di documenti XML basato sulla trasformazione funzionale pure. Tale approccio viene quindi messo a confronto con un approccio procedurale.  
  
## <a name="modifying-an-xml-document"></a>Modifica di un documento XML  
 Una delle attività più comuni richieste a un programmatore XML è la trasformazione di codice XML da una forma a un'altra. Per forma di un documento XML si intende la struttura del documento, che include:  
  
- La gerarchia espressa dal documento.  
  
- I nomi di elementi e attributi.  
  
- I tipi di dati di elementi e attributi.  
  
 In generale, l'approccio più efficace alla trasformazione di codice XML da una forma a un'altra è quello basato sulla trasformazione funzionale pure. In questo approccio l'attività principale del programmatore consiste nel creare una trasformazione che viene applicata all'intero documento XML (o a uno o più nodi definiti rigidamente). La trasformazione funzionale è decisamente la più facile da codificare (una volta che il programmatore ha familiarizzato con l'approccio), consente di ottenere codice più conservabile ed offre spesso maggior compattezza rispetto agli approcci alternativi.  
  
### <a name="xml-functional-transformational-technologies"></a>Tecnologie per la trasformazione funzionale XML  
 Microsoft offre due tecnologie per la trasformazione funzionale utilizzabili con i documenti XML, ovvero XSLT e LINQ to XML. XSLT è supportato nello spazio dei nomi gestito <xref:System.Xml.Xsl> e nell'implementazione COM nativa di MSXML. Sebbene XSLT sia una tecnologia affidabile per la modifica di documenti XML, richiede una certa esperienza in un dominio specializzato, ovvero quello del linguaggio XSLT e delle API che lo supportano.  
  
 LINQ to XML fornisce gli strumenti necessari per codificare trasformazioni funzionali pure in modo espressivo e potente all'interno di codice C# o Visual Basic. Ad esempio, in molti degli esempi della documentazione di LINQ to XML viene usato un approccio funzionale pure. Anche nell'[Esercitazione: Manipolazione di contenuto in un documento WordprocessingML (C#)](../../../../csharp/programming-guide/concepts/linq/shape-of-wordprocessingml-documents.md) LINQ to XML viene usato con un approccio funzionale per modificare le informazioni in un documento di Microsoft Word.  
  
 Per un confronto più completo tra LINQ to XML e altre tecnologie XML Microsoft, vedere [LINQ to XML e altre tecnologie XML](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-vs-other-xml-technologies.md).  
  
 XSLT è lo strumento consigliato per effettuare trasformazioni basate su documenti in cui il documento di origine è caratterizzato da una struttura irregolare. Tuttavia, anche LINQ to XML è in grado di eseguire trasformazioni basate su documenti. Per altre informazioni, vedere [Procedura: Usare annotazioni per trasformare alberi LINQ to XML in uno stile XSLT (C#)](../../../../csharp/programming-guide/concepts/linq/how-to-use-annotations-to-transform-linq-to-xml-trees-in-an-xslt-style.md).  
  
## <a name="see-also"></a>Vedere anche

- [Introduzione alle trasformazioni funzionali pure (C#)](../../../../csharp/programming-guide/concepts/linq/introduction-to-pure-functional-transformations.md)
- [Esercitazione: Manipolazione di contenuto in un documento WordprocessingML (C#)](../../../../csharp/programming-guide/concepts/linq/shape-of-wordprocessingml-documents.md)
- [LINQ to XML e altre tecnologie XML](../../../../csharp/programming-guide/concepts/linq/linq-to-xml-vs-other-xml-technologies.md)
