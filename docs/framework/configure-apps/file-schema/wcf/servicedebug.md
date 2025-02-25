---
title: <serviceDebug>
ms.date: 03/30/2017
ms.assetid: 6d7ea986-f232-49fe-842c-f934d9966889
ms.openlocfilehash: 7b7526dbcbd1948d3d8a27d146efd0462fefaca5
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61788440"
---
# <a name="servicedebug"></a>\<serviceDebug>
Specifica le funzionalità di informazioni di debug e della Guida per un servizio Windows Communication Foundation (WCF).  
  
 \<system.ServiceModel>  
\<behaviors>  
\<serviceBehaviors>  
\<behavior>  
\<serviceDebug>  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<serviceDebug httpHelpPageBinding="String"
              httpHelpPageBindingConfiguration="String"
              httpHelpPageEnabled="Boolean"
              httpHelpPageUrl="Uri"
              httpsHelpPageBinding="String"
              httpsHelpPageBindingConfiguration="String"
              httpsHelpPageEnabled="Boolean"
              httpsHelpPageUrl="Uri"
              includeExceptionDetailInFaults="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|httpHelpPageBinding|Valore stringa che specifica il tipo di associazione da usare quando HTTP viene usato per accedere alla pagina della Guida del servizio.<br /><br /> Verranno supportate sole le associazioni con elementi di associazione interni che supportano <xref:System.ServiceModel.Channels.IReplyChannel?displayProperty=nameWithType>. La proprietà <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> dell'associazione deve inoltre essere <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType>.|  
|httpHelpPageBindingConfiguration|Stringa che specifica il nome dell'associazione specificata nell'attributo `httpHelpPageBinding` che fa riferimento alle informazioni di configurazione aggiuntive di questa associazione. Lo stesso nome deve essere definito nella sezione `<bindings>`.|  
|httpHelpPageEnabled|Valore booleano che controlla se WCF pubblica una pagina della Guida HTML all'indirizzo specificato da di `httpHelpPageUrl` attributo. Il valore predefinito è `true`.<br /><br /> È possibile impostare questa proprietà su `false` per disabilitare la pubblicazione di una pagina della Guida HTML visibile ai browser HTML.<br /><br /> Per garantire che la pagina della Guida HTML sia pubblicata nel percorso controllato dall'attributo `httpHelpPageUrl`, è necessario impostare questo attributo su `true`. Deve inoltre venire soddisfatta anche una delle condizioni seguenti:<br /><br /> -La `httpHelpPageUrl` attributo è un indirizzo assoluto che supporta lo schema del protocollo HTTP.<br />-È un indirizzo di base per il servizio che supporta lo schema del protocollo HTTP.<br /><br /> Sebbene venga generata un'eccezione quando all'attributo `httpHelpPageUrl` viene assegnato un indirizzo assoluto che non supporta lo schema del protocollo HTTP, qualsiasi altro scenario nel quale non viene soddisfatto alcuno dei criteri precedenti comporta il fatto che non venga generata alcuna eccezione e che non sia disponibile alcuna pagina della Guida HTML.|  
|httpHelpPageUrl|URI che specifica l'URL relativo o assoluto basato su HTTPS del file della Guida HTML personalizzato che l'utente vede quando l'endpoint viene visualizzato tramite un browser HTML.<br /><br /> È possibile usare questo attributo per consentire l'uso di un file della Guida HTML personalizzato restituito da una richiesta HTTP/Get, ad esempio, da un browser HTML. Il percorso del file della Guida HTML viene risolto come segue.<br /><br /> 1.  Se il valore di questo attributo è un indirizzo relativo, il percorso del file della Guida HTML è uguale al valore dell'indirizzo di base del servizio che supporta le richieste HTTP, più il valore di questa proprietà.<br />2.  Se il valore di questo attributo è un indirizzo assoluto e supporta le richieste HTTP, il percorso del file della Guida HTML è uguale al valore di questa proprietà.<br />3.  Se il valore di questo attributo è assoluto ma non supporta le richieste HTTP, viene generata un'eccezione.<br /><br /> Questo attributo è valido solo quando la `httpHelpPageEnabled` attributo è `true`.|  
|httpsHelpPageBinding|Valore stringa che specifica il tipo di associazione da usare quando HTTPS viene usato per accedere alla pagina della Guida del servizio.<br /><br /> Verranno supportate sole le associazioni con elementi di associazione interni che supportano <xref:System.ServiceModel.Channels.IReplyChannel>. La proprietà <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> dell'associazione deve inoltre essere <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType>.|  
|httpsHelpPageBindingConfiguration|Stringa che specifica il nome dell'associazione specificata nell'attributo `httpsHelpPageBinding` che fa riferimento alle informazioni di configurazione aggiuntive di questa associazione. Lo stesso nome deve essere definito nella sezione `<bindings>`.|  
|httpsHelpPageEnabled|Valore booleano che controlla se WCF pubblica una pagina della Guida HTML all'indirizzo specificato da di `httpsHelpPageUrl` attributo. Il valore predefinito è `true`.<br /><br /> È possibile impostare questa proprietà su `false` per disabilitare la pubblicazione di una pagina della Guida HTML visibile ai browser HTML.<br /><br /> Per garantire che la pagina della Guida HTML sia pubblicata nel percorso controllato dall'attributo `httpsHelpPageUrl`, è necessario impostare questo attributo su `true`. Deve inoltre venire soddisfatta anche una delle condizioni seguenti:<br /><br /> -La `httpsHelpPageUrl` attributo è un indirizzo assoluto che supporta lo schema del protocollo HTTPS.<br />-È un indirizzo di base per il servizio che supporta lo schema del protocollo HTTPS.<br /><br /> Sebbene venga generata un'eccezione quando all'attributo `httpsHelpPageUrl` viene assegnato un indirizzo assoluto che non supporta lo schema del protocollo HTTPS, qualsiasi altro scenario nel quale non viene soddisfatto alcuno dei criteri precedenti comporta il fatto che non venga generata alcuna eccezione e non sia disponibile alcuna pagina della Guida HTML.|  
|httpsHelpPageUrl|URI che specifica l'URL relativo o assoluto basato su HTTP del file della Guida HTML personalizzato che l'utente vede quando l'endpoint viene visualizzato tramite un browser HTML.<br /><br /> È possibile usare questo attributo per consentire l'uso di un file della Guida HTML personalizzato restituito da una richiesta HTTPS/Get, ad esempio, da un browser HTML. Il percorso del file della Guida HTML viene risolto come segue.<br /><br /> -Se il valore di questa proprietà è un indirizzo relativo, il percorso del file della Guida HTML è uguale al valore dell'indirizzo di base del servizio che supporta le richieste HTTPS, più il valore della proprietà.<br />-Se il valore di questa proprietà è un indirizzo assoluto e supporta le richieste HTTPS, il percorso del file della Guida HTML è il valore di questa proprietà.<br />-Se il valore di questa proprietà è assoluto ma non supporta le richieste HTTPS, viene generata un'eccezione.<br /><br /> Questo attributo è valido solo quando la `httpHelpPageEnabled` attributo è `true`.|  
|includeExceptionDetailInFaults|Valore che specifica se includere informazioni sulle eccezioni gestite nei dettagli sugli errori SOAP restituiti al client a scopo di debug. Il valore predefinito è `false`.<br /><br /> Se si imposta l'attributo su `true`, è possibile abilitare il flusso di informazioni sulle eccezioni gestite al client a fini di debug, nonché la pubblicazione di file di informazioni HTML per gli utenti che accedono al servizio tramite un browser Web. **Attenzione:**  La restituzione ai client di informazioni sulle eccezioni gestite può costituire un rischio per la sicurezza. Questo perché i dettagli delle eccezioni espongono informazioni sull'implementazione interna del servizio che potrebbero venire usate da client non autorizzati.|  
  
### <a name="child-elements"></a>Elementi figlio  
 Nessuno.  
  
### <a name="parent-elements"></a>Elementi padre  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|[\<behavior>](../../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)|Specifica un elemento di comportamento.|  
  
## <a name="remarks"></a>Note  
 L'impostazione `includeExceptionDetailInFaults` al `true` consente al servizio di restituire qualsiasi eccezione generata dal codice dell'applicazione anche se l'eccezione non è dichiarata mediante il <xref:System.ServiceModel.FaultContractAttribute>. Questa impostazione è utile in caso dell'esecuzione il debug di casi dove il server sta generando un'eccezione imprevista. L'utilizzo di questo attributo consente la restituzione di un modulo serializzato dell'eccezione sconosciuta e offre all'utente la possibilità di esaminare l'eccezione in dettaglio.  
  
> [!CAUTION]
>  La restituzione ai client delle informazioni sulle eccezioni gestite può rappresentare un rischio per la sicurezza, poiché i dettagli delle eccezioni espongono informazioni sull'implementazione del servizio interno che potrebbero essere usate da client non autorizzati. A causa dei problemi di sicurezza associati, è consigliabile procedere in tal modo solo negli scenari di debug controllati. Durante la distribuzione dell'applicazione, è necessario impostare `includeExceptionDetailInFaults` su `false`.  
  
 Per informazioni dettagliate sui problemi di sicurezza correlati alle eccezioni gestite, vedere [se si specifica e gestione degli errori in contratti e servizi](../../../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md). Per un esempio di codice, vedere [il comportamento di Debug del servizio](../../../../../docs/framework/wcf/samples/service-debug-behavior.md).  
  
 Per abilitare o disabilitare la pagina della Guida, è inoltre possibile impostare `httpsHelpPageEnabled` e `httpsHelpPageUrl`. Ogni servizio può esporre facoltativamente una pagina della Guida che contiene informazioni sul servizio incluso l'endpoint ottenere WSDL per il servizio. A tale fine, impostare `httpHelpPageEnabled` su `true`. Consente di restituire la pagina della Guida a una richiesta GET all'indirizzo di base del servizio. È possibile modificare questo indirizzo impostando l'attributo `httpHelpPageUrl`. È inoltre possibile rendere l'operazione protetta usando HTTPS anziché HTTP.  
  
 Gli attributi `httpHelpPageBinding` e `httpHelpPageBinding` facoltativi consentono di configurare le associazioni usate per accedere alla pagina Web del servizio. Se non vengono specificati, per l'accesso alla pagina della Guida del servizio verranno usate le associazioni predefinite (`HttpTransportBindingElement` per HTTP e `HttpsTransportBindingElement` per HTTPS) a seconda dei casi. Si noti che non è possibile usare questi attributi con le associazioni WCF incorporate. Sole le associazioni con elementi di associazione interni che supportano xref:System.ServiceModel.Channels.IReplyChannel > saranno supportati. La proprietà <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> dell'associazione deve inoltre essere <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.Configuration.ServiceDebugElement>
- <xref:System.ServiceModel.Description.ServiceDebugBehavior>
- [Specifica e gestione degli errori in contratti e servizi](../../../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md)
- [Gestione di eccezioni ed errori](../../../../../docs/framework/wcf/extending/handling-exceptions-and-faults.md)
- [Comportamento di debug del servizio](../../../../../docs/framework/wcf/samples/service-debug-behavior.md)
