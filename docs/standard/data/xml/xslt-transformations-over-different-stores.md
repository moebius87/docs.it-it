---
title: Trasformazioni XSLT su diversi archivi
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 369850e9-004a-45d2-b5c3-5060d9135adb
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 3e9b8c41602ed180b491ca55816fadf28d6cecd3
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65586509"
---
# <a name="xslt-transformations-over-different-stores"></a>Trasformazioni XSLT su diversi archivi
> [!NOTE]
>  La classe <xref:System.Xml.Xsl.XslTransform> è obsoleta in [!INCLUDE[dnprdnext](../../../../includes/dnprdnext-md.md)]. È possibile eseguire le trasformazioni XSLT (Extensible Stylesheet Language for Transformations) usando la classe <xref:System.Xml.Xsl.XslCompiledTransform>. Per altre informazioni, vedere [Utilizzo della classe XslCompiledTransform](../../../../docs/standard/data/xml/using-the-xslcompiledtransform-class.md) e [Migrazione dalla classe XslTransform](../../../../docs/standard/data/xml/migrating-from-the-xsltransform-class.md).  
  
 Le classi ADO.NET e XML in .NET Framework offrono un modello di programmazione unificato per accedere ai dati. Tali dati sono rappresentati come dati XML, vale a dire testo delimitato da tag e dati relazionali, ovvero tabelle composte da righe e colonne. XML in .NET Framework legge i dati XML da qualsiasi flusso di dati in alberi di nodi DOM (Document Object Model), dove è possibile accedere ai dati a livello di codice, mentre ADO.NET offre gli strumenti per accedere ai dati relazionali e modificarli all'interno di un oggetto <xref:System.Data.DataSet>.  
  
 Il DOM XML fornisce l'accesso ai dati nei documenti XML e nelle classi aggiuntive per scrivere, leggere e spostarsi all'interno dei documenti XML. Queste classi sono supportate nello spazio dei nomi <xref:System.Xml>, che unifica inoltre il DOM XML con i servizi di accesso ai dati forniti da ADO.NET. L'oggetto <xref:System.Xml.XmlDataDocument> fornisce l'accesso relazionale ai dati. Il tipo <xref:System.Xml.XmlDataDocument> associa il codice XML ai dati relazionali in un <xref:System.Data.DataSet> di ADO.NET. Qualsiasi applicazione basata su .NET Framework può usare le classi disponibili nello spazio dei nomi <xref:System.Xml> per accedere ai documenti XML e ai dati relazionali in <xref:System.Xml.XmlDataDocument> e modificarli. Questa implementazione supporta le architetture a più livelli per la raccolta e la distribuzione dei dati. Per altre informazioni, vedere [Integrazione di XML con dati relazionali e ADO.NET](../../../../docs/standard/data/xml/xml-integration-with-relational-data-and-adonet.md).  
  
## <a name="see-also"></a>Vedere anche

- [Implementazione del processore XSLT da parte della classe XslTransform](../../../../docs/standard/data/xml/xsltransform-class-implements-the-xslt-processor.md)
