---
title: <transport> di <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 49462e0a-66e1-463f-b3e1-c83a441673c6
ms.openlocfilehash: 97139b6bea21e4d908c06f5210e54756865d3c46
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61788310"
---
# <a name="transport-of-nettcpbinding"></a>\<transport> di \<netTcpBinding>
Definisce il tipo di requisiti di sicurezza a livello di messaggio per un endpoint configurato con il [ \<netTcpBinding >](../../../../../docs/framework/configure-apps/file-schema/wcf/nettcpbinding.md).  
  
 \<system.ServiceModel>  
\<le associazioni >  
\<netTcpBinding>  
\<binding>  
\<security>  
\<transport>  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<netTcpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential">
      <transport clientCredentialType="None|Windows|Certificate"
                 protectionLevel="None|Sign|EncryptAndSign"
                 sslProtocols="Tls|Tls11|Tls12">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</netTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti attributi, elementi figlio ed elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|clientCredentialType|Facoltativo. Specifica il tipo di credenziale da usare se l'autenticazione client viene eseguita usando la sicurezza del trasporto.<br /><br /> -Il valore predefinito è `Windows`.<br />-L'attributo è di tipo <xref:System.ServiceModel.TcpClientCredentialType>.|  
|protectionLevel|Facoltativo. Definisce il livello di sicurezza del trasporto TCP. La firma dei messaggi riduce il rischio di manomissione da parte di terzi durante il trasferimento. La crittografia fornisce riservatezza a livello di dati durante il trasporto.<br /><br /> Il valore predefinito è `EncryptAndSign`.|  
|sslProtocols|Valore del flag di enumerazione SslProtocols che specifica gli elementi SslProtocol supportati. Il valore predefinito è Tls&#124;Tls11&#124;Tls12.|  
|policyEnforcement|Questa enumerazione specifica il momento in cui deve essere applicato l'oggetto <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>.<br /><br /> 1.  Never – I criteri non vengono mai applicati e la protezione estesa è disabilitata.<br />2.  WhenSupported – I criteri vengono applicati solo se il client supporta la protezione estesa.<br />3.  Always - I criteri vengono sempre applicati. L'autenticazione dei client che non supportano la protezione estesa avrà esito negativo.|  
  
## <a name="clientcredentialtype-attribute"></a>Attributo clientCredentialType  
  
|Value|Descrizione|  
|-----------|-----------------|  
|nessuno|Il client è anonimo. Richiede un certificato per il servizio.|  
|WINDOWS|Specifica l'autenticazione Windows del client mediante la negoziazione SP (negoziazione Kerberos).|  
|Certificato|Il client viene autenticato mediante un certificato. Viene usata la negoziazione SSL ed è necessario un certificato per il servizio.|  
  
## <a name="protectionlevel-attribute"></a>Attributo protectionLevel  
  
|Value|Descrizione|  
|-----------|-----------------|  
|nessuno|Nessuna protezione.|  
|Sign|I messaggi vengono firmati.|  
|EncryptAndSign|-I messaggi vengono crittografati e firmati.|  
  
### <a name="child-elements"></a>Elementi figlio  
 nessuno  
  
### <a name="parent-elements"></a>Elementi padre  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|[\<security>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-nettcpbinding.md)|Specifica le funzionalità di sicurezza del [ \<netTcpBinding >](../../../../../docs/framework/configure-apps/file-schema/wcf/nettcpbinding.md).|  
  
## <a name="remarks"></a>Note  
 Usare la sicurezza del trasporto garantire l'integrità e la riservatezza del messaggio SOAP, nonché l'esecuzione dell'autenticazione reciproca. Se questa modalità di sicurezza viene selezionata per un'associazione, lo stack di canali viene configurato in modo da usare un trasporto protetto nonché proteggere i messaggi SOAP tramite una sicurezza di trasporto quale Windows (Negotiate) o SSL su TCP.  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ServiceModel.TcpTransportSecurity>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.NetTcpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [Protezione di servizi e client](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)
- [Associazioni](../../../../../docs/framework/wcf/bindings.md)
- [Configurazione di associazioni fornite dal sistema](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)
- [Uso di associazioni per configurare servizi e client](../../../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](../../../../../docs/framework/misc/binding.md)
