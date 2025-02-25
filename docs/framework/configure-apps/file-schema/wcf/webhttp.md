---
title: <webHttp>
ms.date: 03/30/2017
ms.assetid: 1f9d0754-d41e-44ce-a298-e51cb3096c64
ms.openlocfilehash: 795e61b9054d2ea9276970988018c50099bcbe17
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61769798"
---
# <a name="webhttp"></a>\<webHttp>
Questo elemento specifica il comportamento <xref:System.ServiceModel.Description.WebHttpBehavior> in un endpoint tramite la configurazione. Questo comportamento, quando viene usato in combinazione con il [ \<webHttpBinding >](../../../../../docs/framework/configure-apps/file-schema/wcf/webhttpbinding.md) associazione standard, attiva il modello di programmazione Web per un servizio Windows Communication Foundation (WCF).  
  
 \<system.ServiceModel>  
\<behaviors>  
\<endpointBehaviors>  
\<behavior>  
\<webHttp>  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<webHttp />
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|automaticFormatSelectionEnabled|Quando questa proprietà è impostata su `true`, l'infrastruttura di WCF determina il formato migliore da usare. La selezione automatica del formato è disabilitata per impostazione predefinita per la compatibilità con le versioni precedenti. La selezione automatica del formato automatica può essere abilitata a livello di codice o tramite la configurazione.|  
|defaultBodyStyle|Specifica lo stile predefinito del corpo dei messaggi restituiti. Per altre informazioni, vedere <xref:System.ServiceModel.Web.WebMessageBodyStyle> e [formattazione HTTP Web WCF](../../../../../docs/framework/wcf/feature-details/wcf-web-http-formatting.md).|  
|defaultOutgoingResponseFormat|Specifica il formato del messaggio di risposta in uscita predefinito per i messaggi. Per altre informazioni, vedere [formattazione HTTP Web WCF](../../../../../docs/framework/wcf/feature-details/wcf-web-http-formatting.md).|  
|faultExceptionEnabled|Ottiene o imposta il flag che specifica se viene generata un'eccezione FaultException quando si verifica un errore interno del server (Codice di stato HTTP: 500).|  
|helpEnabled|Ottiene o imposta un valore che determina se la pagina della Guida è abilitata.|  
  
### <a name="child-elements"></a>Elementi figlio  
 Nessuno.  
  
### <a name="parent-elements"></a>Elementi padre  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|[\<behavior>](../../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)|Specifica l'insieme di comportamenti dell'endpoint.|  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.Configuration.WebHttpElement>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- [Integrazione AJAX e supporto JSON](../../../../../docs/framework/wcf/feature-details/ajax-integration-and-json-support.md)
- [\<webHttpBinding>](../../../../../docs/framework/configure-apps/file-schema/wcf/webhttpbinding.md)
