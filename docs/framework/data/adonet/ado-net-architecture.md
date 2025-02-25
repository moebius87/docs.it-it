---
title: Architettura ADO.NET
ms.date: 03/30/2017
ms.assetid: fcd45b99-ae8f-45ab-8b97-d887beda734e
ms.openlocfilehash: 13f65d0a2daf3b477a9b29c4de84fb359c946201
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/19/2019
ms.locfileid: "65877239"
---
# <a name="adonet-architecture"></a>Architettura ADO.NET
L'elaborazione dei dati è stata sempre basata principalmente su un modello a due livelli basato su connessione. In considerazione dell'utilizzo crescente dell'architettura a più livelli per l'elaborazione dei dati, i programmatori si avvalgono di un approccio disconnesso per ottenere applicazioni con scalabilità maggiore.  
  
## <a name="adonet-components"></a>Componenti di ADO.NET  
 I due componenti principali di [!INCLUDE[ado_orcas_long](../../../../includes/ado-orcas-long-md.md)] per l'accesso e la modifica dei dati sono i provider di dati .NET Framework e <xref:System.Data.DataSet>.  
  
### <a name="net-framework-data-providers"></a>Provider di dati .NET Framework  
 I provider di dati .NET Framework sono componenti espressamente progettati per la modifica dei dati e per un accesso ai dati rapido, di tipo forward-only e di sola lettura. L'oggetto `Connection` fornisce la connettività a un'origine dati mentre l'oggetto `Command` consente di accedere ai comandi di database per restituire e modificare i dati, eseguire stored procedure e inviare o recuperare informazioni relative ai parametri. `DataReader` fornisce un flusso di dati a elevate prestazioni dall'origine dati e infine `DataAdapter` costituisce infine un collegamento tra l'oggetto `DataSet` e l'origine dati. Gli oggetti `DataAdapter` vengono usati da `Command` per eseguire comandi SQL nell'origine dati in modo da caricare dati in `DataSet` e risolvere le differenze relative alle modifiche apportate ai dati in `DataSet` fino all'origine dati. Per altre informazioni, vedere [provider di dati .NET Framework](../../../../docs/framework/data/adonet/data-providers.md) e [recupero e modifica dei dati in ADO.NET](../../../../docs/framework/data/adonet/retrieving-and-modifying-data.md).  
  
### <a name="the-dataset"></a>Il DataSet  
 L'oggetto `DataSet` di ADO.NET è stato espressamente progettato per un accesso ai dati indipendente dall'origine dati. Di conseguenza, può essere usato con diverse origini dati, con dati XML o per la gestione di dati locali dell'applicazione. Il `DataSet` contiene una raccolta di uno o più oggetti <xref:System.Data.DataTable>, costituiti da righe e colonne di dati, nonché da informazioni su chiave primaria, chiave esterna, vincoli e relazioni relative ai dati degli oggetti `DataTable`. Per altre informazioni, vedere [DataSet, DataTable e DataView](../../../../docs/framework/data/adonet/dataset-datatable-dataview/index.md).  
  
 Il diagramma seguente illustra la relazione tra un provider di dati .NET Framework e un `DataSet`.  
  
 ![ADO.Net graphic](../../../../docs/framework/data/adonet/media/ado-1-bpuedev11.png "ado_1_bpuedev11")  
Architettura di ADO.NET  
  
### <a name="choosing-a-datareader-or-a-dataset"></a>Selezione di un DataReader o un DataSet  
 Quando si decide se l'applicazione deve usare un `DataReader` (vedere [recupero di dati mediante DataReader](../../../../docs/framework/data/adonet/retrieving-data-using-a-datareader.md)) o un' `DataSet` (vedere [DataSet, DataTable e DataView](../../../../docs/framework/data/adonet/dataset-datatable-dataview/index.md)), considerare il tipo di funzionalità richieste dall'applicazione. Usare un oggetto `DataSet` per eseguire le seguenti operazioni:  
  
- Memorizzare localmente i dati nella cache dell'applicazione in modo da poterli modificare. Se è necessario solo leggere i risultati di una query, il `DataReader` rappresenta la scelta migliore.  
  
- Eseguire attività remote sui dati tra livelli o da un servizio Web XML.  
  
- Interagire dinamicamente con i dati, associandoli ad esempio a un controllo Windows Form o combinando e correlando dati da più origini.  
  
- Eseguire un'elaborazione estensiva dei dati senza richiedere una connessione aperta all'origine dati, in modo da liberare la connessione per consentirne l'uso da parte di altri client.  
  
 Se le funzionalità fornite dal `DataSet` non sono necessarie, è possibile migliorare le prestazioni dell'applicazione usando il `DataReader` per restituire i dati in modo forward-only e di sola lettura. Anche se il `DataAdapter` Usa la `DataReader` per compilare il contenuto di un `DataSet` (vedere [popolamento di un set di dati da un oggetto DataAdapter](../../../../docs/framework/data/adonet/populating-a-dataset-from-a-dataadapter.md)), utilizzando il `DataReader`, è possibile migliorare le prestazioni liberando memoria che potrebbe essere utilizzati per il `DataSet`ed evitare l'elaborazione è necessaria per creare e compilare il contenuto del `DataSet`.  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 LINQ to DataSet fornisce funzionalità di query e controllo dei tipi in fase di compilazione sui dati memorizzati nella cache in un oggetto DataSet. Consente inoltre di scrivere query in uno dei linguaggi di sviluppo di .NET Framework, ad esempio C# o Visual Basic. Per altre informazioni, vedere [LINQ to DataSet](../../../../docs/framework/data/adonet/linq-to-dataset.md).  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 LINQ to SQL supporta l'esecuzione di query su un modello a oggetti mappato alle strutture dei dati di un database relazionale senza usare un modello concettuale intermedio. Ogni tabella è rappresentata da una classe distinta, che consente di connettere il modello a oggetti allo schema del database relazionale. LINQ to SQL converte le query LINQ del modello a oggetti in Transact-SQL e le invia al database per l'esecuzione. I risultati restituiti dal database vengono quindi riconvertiti in oggetti. Per altre informazioni, vedere [LINQ to SQL](../../../../docs/framework/data/adonet/sql/linq/index.md).  
  
## <a name="adonet-entity-framework"></a>ADO.NET Entity Framework  
 ADO.NET Entity Framework è progettato per consentire agli sviluppatori di creare applicazioni di accesso ai dati tramite programmazione in base a un modello di applicazione concettuale anziché direttamente in base a uno schema di archiviazione relazionale. L'obiettivo è quello di ridurre la quantità di codice e le operazioni di manutenzione necessarie per le applicazioni orientate ai dati. Per altre informazioni, vedere [ADO.NET Entity Framework](../../../../docs/framework/data/adonet/ef/index.md).  
  
## <a name="wcf-data-services"></a>WCF Data Services  
 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] viene usato per distribuire servizi dati nel Web o in una rete Intranet. I dati sono strutturati come entità e relazioni in base alle specifiche di Entity Data Model. I dati distribuiti in questo modello sono indirizzabili tramite il protocollo HTTP standard. Per altre informazioni, vedere [WCF Data Services 4.5](../../../../docs/framework/data/wcf/index.md).  
  
## <a name="xml-and-adonet"></a>XML e ADO.NET  
 ADO.NET sfrutta la potenza del codice XML per fornire un accesso disconnesso ai dati. ADO.NET è stato progettato in stretta associazione con le classi XML in .NET Framework. si tratta di componenti di un'unica architettura.  
  
 Le classi XML in .NET Framework e ADO.NET convergono nel `DataSet` oggetto. È possibile compilare il `DataSet` con dati provenienti da un'origine XML, sia che si tratti di un file che di un flusso XML. È possibile scrivere il `DataSet` sotto forma di XML in conformità con le specifiche W3C (World Wide Web Consortium), includendone lo schema sotto forma di schema XSD (XML Schema Definition Language), indipendentemente dall'origine dati nel `DataSet`. XML, che costituisce il formato di serializzazione nativo del `DataSet`, rappresenta un supporto eccellente per lo spostamento di dati tra i livelli e rende il `DataSet` una soluzione ottimale per le attività in remoto relative al contesto dei dati e dello schema da e per un servizio Web XML. Per altre informazioni, vedere [XML Documents and Data](../../../../docs/standard/data/xml/index.md) (Documenti e dati XML).  
  
## <a name="see-also"></a>Vedere anche

- [Panoramica di ADO.NET](../../../../docs/framework/data/adonet/ado-net-overview.md)
- [Provider gestiti ADO.NET e Centro per sviluppatori di set di dati](https://go.microsoft.com/fwlink/?LinkId=217917)
