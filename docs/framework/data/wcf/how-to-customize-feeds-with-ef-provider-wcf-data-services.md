---
title: 'Procedura: Personalizzare i feed con il Provider di Entity Framework (WCF Data Services)'
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, customizing
- WCF Data Services, customizing feeds
ms.assetid: fd16272e-36f2-415e-850e-8a81f2b17525
ms.openlocfilehash: 9770527f41b4981e63d65f27c409b2ce5583d2cc
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61765492"
---
# <a name="how-to-customize-feeds-with-the-entity-framework-provider-wcf-data-services"></a>Procedura: Personalizzare i feed con il Provider di Entity Framework (WCF Data Services)
[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] consente di personalizzare la serializzazione Atom in una risposta del servizio dati in modo che venga eseguito il mapping delle proprietà di un'entità agli elementi inutilizzati definiti nel protocollo AtomPub. In questo argomento viene illustrato come definire attributi di mapping per i tipi di entità in un modello di dati definito in un file con estensione edmx tramite il provider di Entity Framework. Per altre informazioni, vedere [personalizzazione di Feed](../../../../docs/framework/data/wcf/feed-customization-wcf-data-services.md).  
  
 In questo argomento si modificherà manualmente il file con estensione edmx generato dallo strumento contenente il modello di dati. Il file deve essere modificato manualmente in quanto le estensioni al modello di dati non sono supportate da Entity Designer. Per altre informazioni sul file con estensione edmx generato dagli strumenti di Entity Data Model, vedere [edmx (Entity Framework) Panoramica del File](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100)). Nell'esempio riportato in questo argomento vengono usati il servizio dati Northwind di esempio e le classi del servizio dati client generate automaticamente. Questo servizio e le classi dati client vengono create quando si completa la [Guida rapida di WCF Data Services](../../../../docs/framework/data/wcf/quickstart-wcf-data-services.md).  
  
### <a name="to-manually-modify-the-northwindedmx-file-to-add-feed-customization-attributes"></a>Per modificare manualmente il file Northwind.edmx per aggiungere attributi di personalizzazione dei feed  
  
1. In **Esplora soluzioni**, fare doppio clic il `Northwind.edmx` del file e quindi fare clic su **Apri con**.  
  
2. Nel **Apri con - Northwind** finestra di dialogo **Editor XML**, quindi fare clic su **OK**.  
  
3. Individuare l'elemento `ConceptualModels` e sostituire il tipo di entità `Customers` esistente con l'elemento seguente che contiene gli attributi per il mapping della personalizzazione dei feed:  
  
     [!code-xml[Astoria Custom Feeds#EdmFeedCustomers](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/northwind.csdl#edmfeedcustomers)]  
  
4. Salvare le modifiche e chiudere il file Northwind.edmx.  
  
5. (Facoltativo) Fare clic sul file Northwind edmx e quindi fare clic su **Esegui strumento personalizzato**.  
  
     Il file del livello di oggetti verrà rigenerato, poiché potrebbe essere necessario.  
  
6. Ricompilare il progetto.  
  
## <a name="example"></a>Esempio  
 Nell'esempio precedente per l'URI `http://myservice/Northwind.svc/Customers('ALFKI')` viene restituito il risultato seguente.  
  
 [!code-xml[Astoria Custom Feeds#EdmFeedResult](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/edmfeedresult.xml#edmfeedresult)]  
  
## <a name="see-also"></a>Vedere anche

- [Provider di Entity Framework](../../../../docs/framework/data/wcf/entity-framework-provider-wcf-data-services.md)
