---
title: Configurazione di Internet Information Services 7.0 per Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 1050d395-092e-44d3-b4ba-66be3b039ffb
ms.openlocfilehash: 6962ed1dccca6db2e55554459742adab210585ef
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64655011"
---
# <a name="configuring-internet-information-services-70-for-windows-communication-foundation"></a>Configurazione di Internet Information Services 7.0 per Windows Communication Foundation

Internet Information Services (IIS) 7.0 dispone di una progettazione modulare che consente di installare in modo selettivo i componenti necessari. Questa progettazione si basa sulla nuova tecnologia di componentizzazione basata su manifesti introdotta in [!INCLUDE[wv](../../../../includes/wv-md.md)]. Sono presenti più di 40 componenti delle caratteristiche autonoma di IIS 7.0 che possono essere installati in modo indipendente. I professionisti IT possono così personalizzare con facilità l'installazione in base alle esigenze. In questo argomento illustra come configurare IIS 7.0 per l'uso con Windows Communication Foundation (WCF) e determinare quali componenti sono necessari.

## <a name="minimal-installation-installing-was"></a>Installazione minima: Installazione di WAS
 L'installazione minima dell'intero pacchetto IIS 7.0 consiste nell'installare il servizio di attivazione processo Windows (WAS). È stato è una funzionalità autonoma ed è la sola funzionalità di IIS 7.0 che è disponibile per tutti i [!INCLUDE[wv](../../../../includes/wv-md.md)] i sistemi operativi (Home Basic, Home Premium, Business e Ultimate ed Enterprise).

 Dal Pannello di controllo, fare clic su **i programmi** e quindi fare clic su **o disattivazione delle funzionalità Windows attivare** elencata sotto **programmi e funzionalità**, il componente WAS è riportato di elenco come illustrato nella figura seguente.

 ![Attivare le funzionalità o della finestra disattivata](../../../../docs/framework/wcf/feature-details/media/wcfc-turnfeaturesonoroffs.gif "wcfc_TurnFeaturesOnOrOffs")

 Questa funzionalità presenta i sottocomponenti seguenti:

- Ambiente .NET

- API di configurazione

- Modello di processo

 Se si seleziona solo il nodo radice di WAS, il **modello di processo** sottonodo viene selezionata per impostazione predefinita. Si noti che con questa installazione verrà installato solo il servizio WAS poiché non è disponibile alcun supporto per un server Web.

 Per rendere tutte le operazioni di applicazione ASP.NET o WCF, controllare la **ambiente .NET** casella di controllo. Ciò significa che tutti i componenti WAS sono obbligatori per rendere WCF e ASP.NET in modo corretto. Questi verranno selezionati automaticamente una volta installati tali componenti.

## <a name="iis-70-default-installation"></a>IIS 7.0: Installazione predefinita
 Controllando il **Internet Information Services** funzionalità, alcuni dei nodi secondari vengono contrassegnati automaticamente come illustrato nella figura seguente.

 ![Impostazioni predefinite per le funzionalità di IIS 7.0](../../../../docs/framework/wcf/feature-details/media/wcfc-turningfeaturesonoroff2.gif "wcfc_TurningFeaturesOnOrOff2")

 Questo è l'installazione predefinita di IIS 7.0. Con questa installazione, è possibile utilizzare IIS 7.0 per gestire il contenuto statico (ad esempio pagine HTML e altro contenuto). Tuttavia, non è possibile eseguire le applicazioni ASP.NET o CGI o ospitare servizi WCF.

## <a name="iis-70-installation-with-aspnet-support"></a>IIS 7.0: Installazione con supporto ASP.NET
 È necessario installare ASP.NET per il funzionamento di ASP.NET in IIS 7.0. Dopo aver controllato **ASP.NET**, la schermata dovrebbe essere simile alla figura seguente.

 ![Asp.NET le impostazioni necessarie](../../../../docs/framework/wcf/feature-details/media/wcfc-trunfeaturesonoroff3s.gif "wcfc_TrunFeaturesOnOrOFf3s")

 Questo è l'ambiente minimo per le applicazioni ASP.NET sia WCF lavorare in IIS 7.0.

## <a name="iis-70-installation-with-iis-60-compatibility-components"></a>IIS 7.0: Installazione con componenti compatibilità 6.0 di IIS
 Quando si installa IIS 7.0 in un sistema con Visual Studio 2005 o alcuni altri script di automazione o gli strumenti (ad esempio Adsutil. vbs) che consentono di configurare le applicazioni virtuali che usano API della Metabase di IIS 6.0, assicurarsi di selezionare IIS 6.0 **gli strumenti di Scripting**. Si verifica automaticamente gli altri sottonodi di IIS 6.0 **compatibilità gestione**. La figura seguente mostra la schermata dopo questa operazione viene eseguita:

 ![Impostazioni di compatibilità IIS 6.0 Management](../../../../docs/framework/wcf/feature-details/media/scfc-turnfeaturesonoroff5s.gif "scfc_TurnFeaturesOnOrOff5s")

 Con questa installazione, sono disponibili tutte le operazioni necessarie per usare le funzionalità di IIS 7.0, ASP.NET e WCF e gli esempi disponibili sul Web.

## <a name="request-limits"></a>Limiti di richiesta.
 In [!INCLUDE[wv](../../../../includes/wv-md.md)] con IIS 7 il valore predefinito delle impostazioni `maxUri` e `maxQueryStringSize` è stato modificato. Per impostazione predefinita, il filtro di richiesta in IIS 7.0 consente una lunghezza dell'URL di 4096 caratteri e una lunghezza della stringa di query di 2048 caratteri. Per modificare questi valori predefiniti, aggiungere il codice XML seguente al file App.config:

```xml
 <system.webServer>
    <security>
        <requestFiltering>
            <requestLimits maxUrl="8192" maxQueryString="8192" />
        </requestFiltering>
    </security>
 </system.webServer>
 ```

## <a name="see-also"></a>Vedere anche

- [Architettura di attivazione di WAS](../../../../docs/framework/wcf/feature-details/was-activation-architecture.md)
- [Configurazione di WAS per l'uso con WCF](../../../../docs/framework/wcf/feature-details/configuring-the-wpa--service-for-use-with-wcf.md)
- [Procedura: Installare e configurare componenti di attivazione WCF](../../../../docs/framework/wcf/feature-details/how-to-install-and-configure-wcf-activation-components.md)
- [Funzionalità di hosting di Windows Server AppFabric](https://go.microsoft.com/fwlink/?LinkId=201276)
