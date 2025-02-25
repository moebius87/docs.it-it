---
title: 'Procedura: Qualificare nomi di attributi ed elementi XML'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- qualifying XML names
- qualifying XML elements
- XML namespaces, qualifying elements and names in
ms.assetid: 44719f90-7e15-42e8-a9e2-282287e2b5bf
ms.openlocfilehash: 04e9dd3c135c516fa5554b9b547306337fb6a668
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64755394"
---
# <a name="how-to-qualify-xml-element-and-xml-attribute-names"></a>Procedura: Qualificare nomi di attributi ed elementi XML

Spazi dei nomi XML contenuti nelle istanze del <xref:System.Xml.Serialization.XmlSerializerNamespaces> classe deve essere conforme alla specifica del World Wide Web Consortium (W3C) denominata [Namespaces in XML](https://www.w3.org/TR/REC-xml-names/).

Gli spazi dei nomi XML forniscono un metodo che consente di qualificare i nomi di elementi XML e attributi XML nei documenti XML. Un nome completo è composto da un prefisso e da un nome locale, separati dal carattere di due punti. Il prefisso funge soltanto da segnaposto ed è mappato a un URI che specifica uno spazio dei nomi. La combinazione dello spazio dei nomi URI gestito a livello universale e del nome locale genera un nome univoco.

Tramite la creazione di un'istanza di `XmlSerializerNamespaces` e l'aggiunta delle coppie dello spazio dei nomi all'oggetto, è possibile specificare i prefissi utilizzati in un documento XML.

## <a name="to-create-qualified-names-in-an-xml-document"></a>Per creare nomi completi in un documento XML

1. Creare un'istanza della classe `XmlSerializerNamespaces`.

2. Aggiungere tutte le coppie di prefissi e spazi dei nomi a `XmlSerializerNamespaces`.

3. Applicare l'attributo `System.Xml.Serialization` appropriato a ciascun membro o classe che <xref:System.Xml.Serialization.XmlSerializer> deve serializzare in un documento XML.

    Gli attributi disponibili sono: <xref:System.Xml.Serialization.XmlAnyElementAttribute>, <xref:System.Xml.Serialization.XmlArrayAttribute>, <xref:System.Xml.Serialization.XmlArrayItemAttribute>, <xref:System.Xml.Serialization.XmlAttributeAttribute>, <xref:System.Xml.Serialization.XmlElementAttribute>, <xref:System.Xml.Serialization.XmlRootAttribute> e <xref:System.Xml.Serialization.XmlTypeAttribute>.

4. Impostare la proprietà `Namespace` di ciascun attributo su uno dei valori dello spazio dei nomi da `XmlSerializerNamespaces`.

5. Passare `XmlSerializerNamespaces` al metodo `Serialize` di `XmlSerializer`.

## <a name="example"></a>Esempio

Nell'esempio riportato di seguito viene creato un oggetto `XmlSerializerNamespaces`, a cui vengono aggiunte due coppie di prefisso e spazio dei nomi. Il codice crea un `XmlSerializer` utilizzato per serializzare un'istanza della classe `Books`. Il codice chiama il metodo `Serialize` con `XmlSerializerNamespaces`, consentendo all'XML di contenere spazi dei nomi con prefissi.

```vb
Option Explicit
public class Price
{
    [XmlAttribute(Namespace = "http://www.cpandl.com")]
    public string currency;
    [XmlElement(Namespace = "http://www.cohowinery.com")]
    public decimal price;
}

Option Strict

Imports System
Imports System.IO
Imports System.Xml
Imports System.Xml.Serialization

Public Class Run

    Public Shared Sub Main()
        Dim test As New Run()
        test.SerializeObject("XmlNamespaces.xml")
    End Sub 'Main

    Public Sub SerializeObject(filename As String)
        Dim mySerializer As New XmlSerializer(GetType(Books))
        ' Writing a file requires a TextWriter.
        Dim myWriter As New StreamWriter(filename)

        ' Creates an XmlSerializerNamespaces and adds two
        ' prefix-namespace pairs.
        Dim myNamespaces As New XmlSerializerNamespaces()
        myNamespaces.Add("books", "http://www.cpandl.com")
        myNamespaces.Add("money", "http://www.cohowinery.com")

        ' Creates a Book.
        Dim myBook As New Book()
        myBook.TITLE = "A Book Title"
        Dim myPrice As New Price()
        myPrice.price = CDec(9.95)
        myPrice.currency = "US Dollar"
        myBook.PRICE = myPrice
        Dim myBooks As New Books()
        myBooks.Book = myBook
        mySerializer.Serialize(myWriter, myBooks, myNamespaces)
        myWriter.Close()
    End Sub
End Class

Public Class Books
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public Book As Book
End Class 'Books

<XmlType([Namespace] := "http://www.cpandl.com")> _
Public Class Book

    <XmlElement([Namespace] := "http://www.cpandl.com")> _
    Public TITLE As String
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public PRICE As Price
End Class

Public Class Price
    <XmlAttribute([Namespace] := "http://www.cpandl.com")> _
    Public currency As String
    Public <XmlElement([Namespace] := "http://www.cohowinery.com")> _
        price As Decimal
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.Serialization;

public class Run
{
    public static void Main()
    {
        Run test = new Run();
        test.SerializeObject("XmlNamespaces.xml");
    }
    public void SerializeObject(string filename)
    {
        XmlSerializer mySerializer = new XmlSerializer(typeof(Books));
        // Writing a file requires a TextWriter.
        TextWriter myWriter = new StreamWriter(filename);

        // Creates an XmlSerializerNamespaces and adds two
        // prefix-namespace pairs.
        XmlSerializerNamespaces myNamespaces =
        new XmlSerializerNamespaces();
        myNamespaces.Add("books", "http://www.cpandl.com");
        myNamespaces.Add("money", "http://www.cohowinery.com");

        // Creates a Book.
        Book myBook = new Book();
        myBook.TITLE = "A Book Title";
        Price myPrice = new Price();
        myPrice.price = (decimal) 9.95;
        myPrice.currency = "US Dollar";
        myBook.PRICE = myPrice;
        Books myBooks = new Books();
        myBooks.Book = myBook;
        mySerializer.Serialize(myWriter,myBooks,myNamespaces);
        myWriter.Close();
    }
}

public class Books
{
    [XmlElement(Namespace = "http://www.cohowinery.com")]
    public Book Book;
}

[XmlType(Namespace ="http://www.cpandl.com")]
public class Book
{
    [XmlElement(Namespace = "http://www.cpandl.com")]
    public string TITLE;
    [XmlElement(Namespace ="http://www.cohowinery.com")]
    public Price PRICE;
}
```

## <a name="see-also"></a>Vedere anche

- <xref:System.Xml.Serialization.XmlSerializer>
- [Strumento XML Schema Definition e serializzazione XML](the-xml-schema-definition-tool-and-xml-serialization.md)
- [Introduzione alla serializzazione XML](introducing-xml-serialization.md)
- [Classe XmlSerializer](xref:System.Xml.Serialization.XmlSerializer)
- [Attributi per il controllo della serializzazione XML](attributes-that-control-xml-serialization.md)
- [Procedura: Specificare un nome di elemento alternativo per un Stream XML](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [Procedura: Serializzare un oggetto](how-to-serialize-an-object.md)
- [Procedura: Deserializzare un oggetto](how-to-deserialize-an-object.md)
