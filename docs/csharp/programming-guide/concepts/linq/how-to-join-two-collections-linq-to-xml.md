---
title: 'Procedura: Unire due raccolte (LINQ to XML) (C#)'
ms.date: 07/20/2015
ms.assetid: 7b817ede-911a-4cff-9dd3-639c3fc228c9
ms.openlocfilehash: 893966f3b803b92efbc89a65870623f10195c85f
ms.sourcegitcommit: 155012a8a826ee8ab6aa49b1b3a3b532e7b7d9bd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66485374"
---
# <a name="how-to-join-two-collections-linq-to-xml-c"></a>Procedura: Unire due raccolte (LINQ to XML) (C#)
Un elemento o un attributo di un documento XML può talvolta fare riferimento a un altro elemento o attributo. Ad esempio, il documento XML [File XML di esempio: Customers e Orders (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-customers-and-orders-linq-to-xml-2.md) contiene un elenco di clienti e un elenco di ordini. Ogni elemento `Customer` contiene un attributo `CustomerID`. Ogni elemento `Order` contiene un elemento `CustomerID`. L'elemento `CustomerID` di ciascun ordine si riferisce all'attributo `CustomerID` di un cliente.  
  
 L'argomento [File XSD di esempio: Customers and Orders ](../../../../csharp/programming-guide/concepts/linq/sample-xsd-file-customers-and-orders1.md) contiene uno schema XSD che può essere usato per convalidare questo documento. Lo schema usa le funzionalità `xs:key` e `xs:keyref` di XSD per stabilire che l'attributo `CustomerID` dell'elemento `Customer` è una chiave e per stabilire una relazione tra l'elemento `CustomerID` in ogni elemento `Order` e l'attributo `CustomerID` in ogni elemento `Customer`.  
  
 Con [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] è possibile sfruttare questa relazione usando la clausola `join`.  
  
 Notare che, poiché non è disponibile nessun indice, una tale operazione di join sarà caratterizzata da prestazioni ridotte in fase di esecuzione.  
  
 Per altre informazioni su `join`, vedere [Operazioni Join (C#)](../../../../csharp/programming-guide/concepts/linq/join-operations.md).  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente gli elementi `Customer` vengono uniti agli elementi `Order` tramite join e viene generato un nuovo documento XML che include l'elemento `CompanyName` negli ordini.  
  
 Prima di eseguire la query, l'esempio convalida la conformità del documento allo schema descritto in [File XSD di esempio: Customers e Orders](../../../../csharp/programming-guide/concepts/linq/sample-xsd-file-customers-and-orders1.md). Tale convalida assicura la corretta esecuzione della clausola di join.  
  
 La query recupera prima tutti gli elementi `Customer` e quindi li unisce agli elementi `Order` tramite join. Vengono selezionati solo gli ordini relativi ai clienti il cui valore di `CustomerID` è maggiore di "K". Viene quindi proiettato un nuovo elemento `Order` che contiene le informazioni del cliente all'interno di ciascun ordine.  
  
 Nell'esempio viene usato il documento XML seguente: [File XML di esempio: Customers and Orders (LINQ to XML)](../../../../csharp/programming-guide/concepts/linq/sample-xml-file-customers-and-orders-linq-to-xml-2.md).  
  
 L'esempio usa lo schema XSD seguente: [File XSD di esempio: Customers e Orders](../../../../csharp/programming-guide/concepts/linq/sample-xsd-file-customers-and-orders1.md).  
  
 Notare che le unioni tramite join eseguite in questo modo comportano problemi. I join vengono eseguiti tramite una ricerca lineare, pertanto l'assenza di tabelle hash o indici influisce negativamente sulle prestazioni.  
  
```csharp  
XmlSchemaSet schemas = new XmlSchemaSet();  
schemas.Add("", "CustomersOrders.xsd");  
  
Console.Write("Attempting to validate, ");  
XDocument custOrdDoc = XDocument.Load("CustomersOrders.xml");  
  
bool errors = false;  
custOrdDoc.Validate(schemas, (o, e) =>  
                     {  
                         Console.WriteLine("{0}", e.Message);  
                         errors = true;  
                     });  
Console.WriteLine("custOrdDoc {0}", errors ? "did not validate" : "validated");  
  
if (!errors)  
{  
    // Join customers and orders, and create a new XML document with  
    // a different shape.  
  
    // The new document contains orders only for customers with a  
    // CustomerID > 'K'  
    XElement custOrd = custOrdDoc.Element("Root");  
    XElement newCustOrd = new XElement("Root",  
        from c in custOrd.Element("Customers").Elements("Customer")  
        join o in custOrd.Element("Orders").Elements("Order")  
                   on (string)c.Attribute("CustomerID") equals  
                      (string)o.Element("CustomerID")  
        where ((string)c.Attribute("CustomerID")).CompareTo("K") > 0  
        select new XElement("Order",  
            new XElement("CustomerID", (string)c.Attribute("CustomerID")),  
            new XElement("CompanyName", (string)c.Element("CompanyName")),  
            new XElement("ContactName", (string)c.Element("ContactName")),  
            new XElement("EmployeeID", (string)o.Element("EmployeeID")),  
            new XElement("OrderDate", (DateTime)o.Element("OrderDate"))  
        )  
    );  
    Console.WriteLine(newCustOrd);  
}  
```  
  
 L'output del codice è il seguente:  
  
```  
Attempting to validate, custOrdDoc validated  
<Root>  
  <Order>  
    <CustomerID>LAZYK</CustomerID>  
    <CompanyName>Lazy K Kountry Store</CompanyName>  
    <ContactName>John Steel</ContactName>  
    <EmployeeID>1</EmployeeID>  
    <OrderDate>1997-03-21T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LAZYK</CustomerID>  
    <CompanyName>Lazy K Kountry Store</CompanyName>  
    <ContactName>John Steel</ContactName>  
    <EmployeeID>8</EmployeeID>  
    <OrderDate>1997-05-22T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>1</EmployeeID>  
    <OrderDate>1997-06-25T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>8</EmployeeID>  
    <OrderDate>1997-10-27T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>6</EmployeeID>  
    <OrderDate>1997-11-10T00:00:00</OrderDate>  
  </Order>  
  <Order>  
    <CustomerID>LETSS</CustomerID>  
    <CompanyName>Let's Stop N Shop</CompanyName>  
    <ContactName>Jaime Yorres</ContactName>  
    <EmployeeID>4</EmployeeID>  
    <OrderDate>1998-02-12T00:00:00</OrderDate>  
  </Order>  
</Root>  
```  
