---
title: 'Procedura: Popolare un albero XML con XmlWriter (LINQ to XML) (Visual Basic)'
ms.date: 07/20/2015
ms.assetid: 5792a0eb-94ee-440d-b601-58cca8c0ee0b
ms.openlocfilehash: de020444950695e27b9d840dac41fab74b9c7eba
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61789077"
---
# <a name="how-to-populate-an-xml-tree-with-an-xmlwriter-linq-to-xml-visual-basic"></a>Procedura: Popolare un albero XML con XmlWriter (LINQ to XML) (Visual Basic)
Per popolare un albero XML, è possibile usare <xref:System.Xml.Linq.XContainer.CreateWriter%2A> per creare <xref:System.Xml.XmlWriter>e quindi scrivere in <xref:System.Xml.XmlWriter>. L'albero XML viene popolato con tutti i nodi scritti in <xref:System.Xml.XmlWriter>.  
  
 Questo metodo viene in genere impiegato quando si usa [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] con un'altra classe che prevede di scrivere in un oggetto <xref:System.Xml.XmlWriter>, ad esempio <xref:System.Xml.Xsl.XslCompiledTransform>.  
  
## <a name="example"></a>Esempio  
 Un possibile utilizzo di <xref:System.Xml.Linq.XContainer.CreateWriter%2A> è il richiamo di una trasformazione XSLT. In questo esempio viene creato un albero XML da cui viene creato un oggetto <xref:System.Xml.XmlReader>. Viene quindi creato un nuovo documento e infine viene creato un oggetto <xref:System.Xml.XmlWriter> per scrivere nel nuovo documento. Viene quindi richiamata la trasformazione XSLT, passando <xref:System.Xml.XmlReader> e <xref:System.Xml.XmlWriter>. Dopo il completamento della trasformazione, il nuovo albero XML viene popolato con i relativi risultati.  
  
```vb  
Dim xslMarkup As XDocument = _  
    <?xml version='1.0'?>   
    <xsl:stylesheet xmlns:xsl='http://www.w3.org/1999/XSL/Transform' version='1.0'>  
        <xsl:template match='/Parent'>  
            <Root>  
                <C1>  
                    <xsl:value-of select='Child1'/>  
                </C1>  
                <C2>  
                    <xsl:value-of select='Child2'/>  
                </C2>  
            </Root>  
        </xsl:template>  
    </xsl:stylesheet>  
  
Dim xmlTree As XDocument = _  
    <?xml version='1.0'?>  
    <Parent>  
        <Child1>Child1 data</Child1>  
        <Child2>Child2 data</Child2>  
    </Parent>  
  
Dim newTree As XDocument = New XDocument()  
Using writer As XmlWriter = newTree.CreateWriter()  
    ' Load the style sheet.  
    Dim xslt As XslCompiledTransform = New XslCompiledTransform()  
    xslt.Load(xslMarkup.CreateReader())  
  
    ' Execute the transformation and output the results to a writer.  
    xslt.Transform(xmlTree.CreateReader(), writer)  
End Using  
  
Console.WriteLine(newTree)  
```  
  
 Questo esempio produce il seguente output:  
  
```xml  
<Root>  
  <C1>Child1 data</C1>  
  <C2>Child2 data</C2>  
</Root>  
```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Xml.Linq.XContainer.CreateWriter%2A>
- <xref:System.Xml.XmlWriter>
- <xref:System.Xml.Xsl.XslCompiledTransform>
- [Creazione di alberi XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/creating-xml-trees.md)
