---
title: Elemento <idn> (impostazioni URI)
ms.date: 03/30/2017
ms.assetid: 16c8e869-1791-4cf5-9244-3d3c738f60ec
ms.openlocfilehash: 369decf8551c76293ca513b8a3e58b4142a74773
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64592754"
---
# <a name="idn-element-uri-settings"></a>\<IDN > (impostazioni Uri)
Specifica se l'analisi IDN (Internationalized Domain Name) viene applicato a un nome di dominio.  
  
## <a name="schema-hierarchy"></a>Gerarchia dello schema  
 [Elemento \<configuration>](../../../../../docs/framework/configure-apps/file-schema/configuration-element.md)  
  
 [\<URI > (impostazioni Uri)](../../../../../docs/framework/configure-apps/file-schema/network/uri-element-uri-settings.md)  
  
 [\<idn>](../../../../../docs/framework/configure-apps/file-schema/network/idn-element-uri-settings.md)  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<idn  
  enabled="All|AllExceptIntranet|None"  
/>  
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|**Elemento**|**Descrizione**|  
|-----------------|---------------------|  
|`enabled`|Specifica che se l'analisi IDN (Internationalized Domain Name) viene applicato a un nome di dominio il valore predefinito è none.|  
  
### <a name="child-elements"></a>Elementi figlio  
 nessuno  
  
### <a name="parent-elements"></a>Elementi padre  
  
|**Elemento**|**Descrizione**|  
|-----------------|---------------------|  
|[uri](../../../../../docs/framework/configure-apps/file-schema/network/uri-element-uri-settings.md)|Contiene le impostazioni che specificano come .NET Framework gestisce gli indirizzi web espressi tramite uniform resource identifier (URI).|  
  
## <a name="remarks"></a>Note  
 L'oggetto esistente <xref:System.Uri> classe è stata estesa in .NET Framework 3.5. 3.0 SP1 e 2.0 SP1 con supporto per gli identificatori IRI (International Resource) e IDN (Internationalized Domain nomi). Gli utenti correnti non noteranno cambiamenti rispetto al comportamento di .NET Framework 2.0 a meno che non consentono in modo specifico gli IRI e IDN supportano. Questo garantisce la compatibilità delle applicazioni con le versioni precedenti di .NET Framework.  
  
 Per abilitare il supporto per IRI, sono necessarie le due modifiche seguenti:  
  
1. Aggiungere la riga seguente al file Machine. config nella directory di .NET Framework 2.0  
  
    ```xml  
    <section name="uri" type="System.Configuration.UriSection, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />  
    ```  
  
2. Specificare se si desidera applicare l'analisi IDN (Internationalized Domain Name) al nome di dominio e se devono essere applicati le regole di analisi IRI. A questo scopo, è possibile usare il file machine.config o il file app.config.  
  
 Esistono tre possibili valori per IDN a seconda del server DNS che vengono usati:  
  
- IDN abilitato = All  
  
     Questo valore convertirà qualsiasi nome di dominio Unicode negli equivalenti Punycode (nomi IDN).  
  
- IDN abilitato = AllExceptIntranet  
  
     Questo valore convertirà tutti i nomi di dominio Unicode non nella Intranet locale per l'utilizzo degli equivalenti Punycode (nomi IDN). In questo caso, per gestire nomi internazionali sulla rete Intranet locale, i server DNS utilizzati per la rete Intranet devono supportare la risoluzione dei nomi Unicode.  
  
- IDN abilitato = nessuno  
  
     Questo valore non convertirà qualsiasi nome di dominio Unicode per l'utilizzo di Punycode. Questo è il valore predefinito che è coerenza con il comportamento di .NET Framework 2.0.  
  
 L'abilitazione degli IDN comporta la conversione di tutte le etichette Unicode in un nome di dominio nei rispettivi equivalenti Punycode. I nomi Punycode contengono solo caratteri ASCII e iniziano sempre con il prefisso "xn--". Questo avviene per supportare i server DNS esistenti in Internet, in quanto la maggior parte dei server DNS supporta solo caratteri ASCII. Vedere il documento RFC 3940.  
  
### <a name="configuration-files"></a>File di configurazione  
 Questo elemento può essere usato nel file di configurazione dell'applicazione o nel file di configurazione del computer (Machine.config).  
  
## <a name="example"></a>Esempio  
  
### <a name="description"></a>Descrizione  
 Nell'esempio seguente mostra una configurazione usata dal <xref:System.Uri> classe per supportare l'analisi IRI e IDN nomi.  
  
### <a name="code"></a>Codice  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All" />  
    <iriParsing enabled="true" />  
  </uri>  
</configuration>  
```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Configuration.IdnElement?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection?displayProperty=nameWithType>
- [Schema delle impostazioni di rete](../../../../../docs/framework/configure-apps/file-schema/network/index.md)
