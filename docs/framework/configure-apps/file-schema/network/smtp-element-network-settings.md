---
title: Elemento <smtp> (impostazioni di rete)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/mailSettings/smtp
- http://schemas.microsoft.com/.NetConfiguration/v2.0#smtp
helpviewer_keywords:
- <smtp> element
- smtp element
ms.assetid: 220b0329-e384-4e0c-86b4-0945ad17efd9
ms.openlocfilehash: 1b5f7406f995a86f0a192dbf3249c067dff570ea
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61674402"
---
# <a name="smtp-element-network-settings"></a>\<SMTP > (impostazioni di rete)
Consente di configurare il formato di consegna, il metodo di recapito e dall'indirizzo per l'invio di messaggi di posta elettronica.  
  
 \<configuration>  
\<system.net>  
\<mailSettings>  
\<smtp>  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<smtp  
  deliveryFormat="format"  
  deliveryMethod="method"  
  from="from address">
    <specifiedPickupDirectory>...</specifiedPickupDirectory>  
    <network>...</network>  
</smtp>  
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|`deliveryFormat`|Specifica il formato di recapito messaggi di posta elettronica in uscita. I valori accettabili sono SevenBit e International.|  
|`deliveryMethod`|Specifica il metodo di recapito messaggi di posta elettronica. I valori accettabili sono SpecifiedPickupDirectory, PickupDirectoryFromIis e rete.|  
|`from`|Specifica l'indirizzo del mittente per messaggi di posta elettronica in uscita.|  
  
### <a name="child-elements"></a>Elementi figlio  
  
|Attributo|Descrizione|  
|---------------|-----------------|  
|`specifiedPickupDirectory`|Configura la directory locale per un server SMTP Simple Mail Transport Protocol ().|  
|`network`|Consente di configurare le opzioni di rete per un server SMTP esterno.|  
  
### <a name="parent-elements"></a>Elementi padre  
  
|**Elemento**|**Descrizione**|  
|-----------------|---------------------|  
|[Elemento \<mailSettings> (impostazioni di rete)](../../../../../docs/framework/configure-apps/file-schema/network/mailsettings-element-network-settings.md)|Consente di configurare le opzioni di invio della posta elettronica.|  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente specifica i parametri appropriati di SMTP per inviare posta elettronica usando le credenziali di rete predefinite.  
  
```xml  
<configuration>  
  <system.net>  
    <mailSettings>  
      <smtp deliveryMethod="Network" deliveryFormat="SevenBit"  from="ben@contoso.com">  
        <network  
          host="localhost"  
          port="25"  
          defaultCredentials="true"  
        />  
      </smtp>  
    </mailSettings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Net.Configuration.SmtpSection?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpDeliveryFormat>
- <xref:System.Net.Mail.SmtpDeliveryMethod>
- [Schema delle impostazioni di rete](../../../../../docs/framework/configure-apps/file-schema/network/index.md)
