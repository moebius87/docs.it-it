---
title: Elemento < NetFx40_LegacySecurityPolicy >
ms.date: 03/30/2017
helpviewer_keywords:
- <NetFx40_LegacySecurityPolicy> element
- NetFx40_LegacySecurityPolicy element
ms.assetid: 07132b9c-4a72-4710-99d7-e702405e02d4
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: e5bfa5449ece1b24d4f47fe3e77e36b26bbe430c
ms.sourcegitcommit: d8ebe0ee198f5d38387a80ba50f395386779334f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689839"
---
# <a name="netfx40legacysecuritypolicy-element"></a>\<NetFx40_LegacySecurityPolicy > elemento

Specifica se il runtime usa i criteri di sicurezza per l'accesso di codice legacy.

\<configuration>\
\<runtime>\
\<NetFx40_LegacySecurityPolicy>

## <a name="syntax"></a>Sintassi

```xml
<NetFx40_LegacySecurityPolicy
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Attributi ed elementi

Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.

### <a name="attributes"></a>Attributi

|Attributo|Descrizione|
|---------------|-----------------|
|`enabled`|Attributo obbligatorio.<br /><br /> Specifica se il runtime Usa criteri CAS legacy.|

## <a name="enabled-attribute"></a>Attributo enabled

|Valore|Descrizione|
|-----------|-----------------|
|`false`|Il runtime non usa criteri CAS legacy. Questa è l'impostazione predefinita.|
|`true`|Il runtime Usa criteri CAS legacy.|

### <a name="child-elements"></a>Elementi figlio

Nessuno.

### <a name="parent-elements"></a>Elementi padre

|Elemento|Descrizione|
|-------------|-----------------|
|`configuration`|Elemento radice in ciascun file di configurazione usato in Common Language Runtime e nelle applicazioni .NET Framework.|
|`runtime`|Contiene informazioni sulle opzioni di inizializzazione in fase di esecuzione.|

## <a name="remarks"></a>Note

In .NET Framework versione 3.5 e versioni precedenti, il criterio CAS è sempre attivo. In .NET Framework 4, è necessario abilitare il criterio CAS.

Questi criteri sono specifica della versione. Criteri personalizzati di autorità di certificazione presenti nelle versioni precedenti di .NET Framework devono essere specificati nuovamente in .NET Framework 4.

Applicando la `<NetFx40_LegacySecurityPolicy>` elemento a un assembly .NET Framework 4 non influisce sul [codice SecurityTransparent](../../../../../docs/framework/misc/security-transparent-code.md); le regole di trasparenza vengono mantenuti.

> [!IMPORTANT]
> Applicando il `<NetFx40_LegacySecurityPolicy>` elemento può comportare riduzioni significative delle prestazioni per gli assembly di immagini native creati dal [generatore di immagini Native (Ngen.exe)](../../../../../docs/framework/tools/ngen-exe-native-image-generator.md) che non sono installati nel [cache assembly globali ](../../../../../docs/framework/app-domains/gac.md). La riduzione delle prestazioni è causato dall'impossibilità di runtime di caricare gli assembly come le immagini native quando è applicato l'attributo, determinando che ne caricati gli assembly come just-in-time.

> [!NOTE]
> Se si specifica una versione di .NET Framework di destinazione precedente a .NET Framework 4 nelle impostazioni del progetto per il progetto di Visual Studio, le autorità di certificazione criteri saranno abilitati, inclusi i criteri personalizzati di autorità di certificazione specificata per tale versione. Tuttavia, non sarà in grado di utilizzare i membri e i nuovi tipi di .NET Framework 4. È anche possibile specificare una versione precedente di .NET Framework usando il [ \<supportedRuntime > elemento](../../../../../docs/framework/configure-apps/file-schema/startup/supportedruntime-element.md) nello schema delle impostazioni di avvio nel [file di configurazione dell'applicazione](../../../../../docs/framework/configure-apps/index.md).

> [!NOTE]
> Sintassi dei file di configurazione è tra maiuscole e minuscole. È necessario usare la sintassi come specificato nelle sezioni di esempio e sintassi.

## <a name="configuration-file"></a>File di configurazione

Questo elemento può essere usato solo nel file di configurazione dell'applicazione.

## <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato come abilitare i criteri CAS legacy per un'applicazione.

```xml
<configuration>
   <runtime>
      <NetFx40_LegacySecurityPolicy enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Vedere anche

- [Schema delle impostazioni di runtime](../../../../../docs/framework/configure-apps/file-schema/runtime/index.md)
- [Schema dei file di configurazione](../../../../../docs/framework/configure-apps/file-schema/index.md)
