---
title: <bindings>
ms.date: 01/22/2018
ms.assetid: b62cd369-5409-4030-8490-9759a462dd3a
ms.openlocfilehash: 479941593b1abefe637525703140b02917c6692b
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61704440"
---
# <a name="bindings"></a>\<le associazioni >

È possibile usare il `bindings` elemento per configurare una raccolta di associazioni standard e personalizzate per Windows Communication Foundation (WCF). Ogni voce è un elemento `binding` che può essere identificato dal relativo `name` univoco. I servizi usano le associazioni collegandole mediante l'oggetto `name`. A partire da [!INCLUDE[netfx40_short](../../../../../includes/netfx40-short-md.md)], non è necessario che le associazioni e i comportamenti dispongano di un nome. Per altre informazioni sulla configurazione predefinita e associazioni privi di nome e i comportamenti, vedere [Simplified Configuration](../../../../../docs/framework/wcf/simplified-configuration.md) e [Simplified Configuration for WCF Services](../../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md).  
  
## <a name="system-provided-bindings"></a>Associazioni fornite dal sistema
 
 Le associazioni fornite dal sistema nascondono la complessità dello stack di messaggistica WCF. Le applicazioni che usano associazioni fornite dal sistema non richiedono il controllo completo sullo stack. Gli attributi esposti in ciascuna associazione fornita dal sistema sono quelli più appropriati per lo scenario di utilizzo gestito dall'associazione.  
  
 La sezione di configurazione di ciascuna associazione fornita dal sistema può definire varie configurazioni usate per configurare l'associazione. Ogni configurazione è identificata da un nome univoco.  
  
 Non è possibile aggiungere elementi o attributi a un'associazione fornita dal sistema. A tale scopo, è necessario implementare un'associazione personalizzata come descritto nel [associazioni personalizzate](#custom-bindings) sezione. È possibile definire un'associazione personalizzata che riproduce un fornita dal sistema di associazione perfettamente e aggiunge alcune impostazioni che vuole che l'applicazione utente abbia il controllo.  
  
 Per un elenco delle associazioni fornite dal sistema, vedere [System-provided Bindings](../../../../../docs/framework/wcf/system-provided-bindings.md).  
  
## <a name="custom-bindings"></a>Associazioni personalizzate

 Le associazioni personalizzate forniscono il controllo completo dello stack dei messaggi WCF. Un'associazione singola definisce lo stack dei messaggi specificando gli elementi di configurazione per gli elementi dello stack nell'ordine in cui vengono visualizzati nello stack. Ogni elemento definisce e configura un elemento dello stack. In ogni associazione personalizzata deve essere presente un solo elemento `transport`. Senza questo elemento, lo stack dei messaggi è incompleto.  
  
 L'ordine in cui gli elementi vengono visualizzati nello stack è importante, perché è l'ordine in cui le operazioni vengono applicate al messaggio. L'ordine necessario per gli elementi dello stack è il seguente:  
  
1. Transazioni (facoltativo)  
  
2. Reliable messaging (facoltativo)  
  
3. Sicurezza (facoltativo)  
  
4. Codificatore  
  
5. Trasporto  
  
 Le associazioni personalizzate sono identificate dal relativo attributo `name`. Per altre informazioni sulle associazioni personalizzate, vedere [associazioni personalizzate](../../../../../docs/framework/wcf/extending/custom-bindings.md).  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.Configuration.BindingsSection?displayProperty=nameWithType>  
- <xref:System.ServiceModel.Channels.Binding?displayProperty=nameWithType>  
- <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType>  
- [Associazioni](../../../../../docs/framework/wcf/bindings.md)  
- [Associazioni personalizzate](../../../../../docs/framework/wcf/extending/custom-bindings.md)  
- [\<customBinding>](../../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)  
- [\<binding>](../../../../../docs/framework/misc/binding.md)
