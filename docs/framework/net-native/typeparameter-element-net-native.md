---
title: <TypeParameter> Elemento (.NET Native)
ms.date: 03/30/2017
ms.assetid: d37bb1b7-1ddc-4c6d-8ecf-583f804a2479
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b03c87c70fa1bfcd331f468d369632f4164300bb
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61982462"
---
# <a name="typeparameter-element-net-native"></a>\<TypeParameter > elemento (.NET Native)
Applica i criteri al tipo rappresentato da un argomento tipo passato a un metodo.  
  
## <a name="syntax"></a>Sintassi  
  
```xml  
<Parameter Name="parameter_name"  
           Activate="policy_type"  
           Browse="policy_type"  
           Dynamic="policy_type"  
           Serialize="policy_type"  
           DataContractSerializer="policy_type"  
           DataContractJsonSerializer="policy_type"  
           XmlSerializer="policy_type"  
           MarshalObject="policy_type"  
           MarshalDelegate="policy_type"  
           MarshalStructure="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>Attributi ed elementi  
 Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.  
  
### <a name="attributes"></a>Attributi  
  
|Attributo|Tipo di attributo|Descrizione|  
|---------------|--------------------|-----------------|  
|`Name`|Generale|Attributo obbligatorio. Il nome del parametro di tipo <xref:System.Type>. Ad esempio, per la firma del metodo `Type.GetInterfaceMap(Type interfaceType)`, il valore dell'attributo `Name` è "interfaceType".|  
|`Activate`|Reflection|Attributo facoltativo. Controlla l'accesso in fase di esecuzione ai costruttori per abilitare l'attivazione di istanze.|  
|`Browse`|Reflection|Attributo facoltativo. Controlla le query per le informazioni sugli elementi di programma, ma non abilita l'accesso in fase di esecuzione.|  
|`Dynamic`|Reflection|Attributo facoltativo. Controlla l'accesso in fase di esecuzione a tutti i membri dei tipi, inclusi costruttori, metodi, campi, proprietà ed eventi, per abilitare la programmazione dinamica.|  
|`Serialize`|Serializzazione|Attributo facoltativo. Controlla l'accesso in fase di esecuzione a costruttori, campi e proprietà per abilitare la serializzazione e la deserializzazione delle istanze del tipo da parte di librerie quali il serializzatore JSON di Newtonsoft.|  
|`DataContractSerializer`|Serializzazione|Attributo facoltativo. Controlla i criteri per la serializzazione che usano la classe <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>.|  
|`DataContractJsonSerializer`|Serializzazione|Attributo facoltativo. Controlla i criteri per la serializzazione JSON che usano la classe <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>.|  
|`XmlSerializer`|Serializzazione|Attributo facoltativo. Controlla i criteri per la serializzazione XML che usano la classe <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>.|  
|`MarshalObject`|Interoperabilità|Attributo facoltativo. Controlla i criteri per effettuare il marshalling dei tipi di riferimento a Windows Runtime e COM.|  
|`MarshalDelegate`|Interoperabilità|Attributo facoltativo. Controlla i criteri per effettuare il marshalling dei tipi delegati come puntatori a funzioni al codice nativo.|  
|`MarshalStructure`|Interoperabilità|Attributo facoltativo. Controlla i criteri per il marshalling dei tipi di valore al codice nativo.|  
  
## <a name="name-attribute"></a>Name (attributo)  
  
|Value|Descrizione|  
|-----------|-----------------|  
|*parameter_name*|Il nome del parametro di tipo <xref:System.Type>. Ad esempio, per la firma del metodo `Type.GetInterfaceMap(Type interfaceType)`, il valore dell'attributo `Name` è "interfaceType".|  
  
## <a name="all-other-attributes"></a>Tutti gli altri attributi  
  
|Value|Descrizione|  
|-----------|-----------------|  
|*policy_setting*|L'impostazione da applicare a questo tipo di criteri. I valori consentiti sono `All`, `Public`, `PublicAndInternal`, `Required Public`, `Required PublicAndInternal` e `Required All`. Per altre informazioni, vedere [Runtime Directive Policy Settings](../../../docs/framework/net-native/runtime-directive-policy-settings.md) (Impostazioni dei criteri delle direttive di runtime).|  
  
### <a name="child-elements"></a>Elementi figlio  
 Nessuno.  
  
### <a name="parent-elements"></a>Elementi padre  
  
|Elemento|Descrizione|  
|-------------|-----------------|  
|[\<Method>](../../../docs/framework/net-native/method-element-net-native.md)|Applica i criteri di reflection di runtime a un costruttore o a un metodo.|  
  
## <a name="remarks"></a>Note  
 L'elemento `<TypeParameter>` è simile all'elemento [\<Parameter>](../../../docs/framework/net-native/parameter-element-net-native.md), ma può essere applicato solo a parametri di tipo <xref:System.Type>. Applica i criteri a qualsiasi tipo rappresentato al runtime dall'argomento Type specificato dall'attributo `Name`.  
  
 Ad esempio, il serializzatore JSON di NewtonSoft include un metodo statico `JsonConvert.DeserializeObject(String value, Type type)`. Le seguenti direttive di reflection:  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Type Name="Newtonsoft.Json.JsonConvert" >  
      <Method Name="DeserializeObject">  
         <GenericParameter Name="type" Serialize="Required All" />  
      </Method>  
   </Type>  
</Directives>  
```  
  
 specificano che i metadati per il tipo di runtime rappresentati dall'argomento `type` devono essere resi disponibili per la serializzazione. Se queste direttive di runtime vengono applicate a un progetto che include il seguente codice sorgente:  
  
```csharp  
Type t = typeof(StockQuote);  
Object obj = JsonConvert.DeserializeObject(data, t);  
```  
  
 le direttive di reflection rendono disponibili i metadati per il tipo `StockQuote` per il serializzatore JSON di NewtonSoft al runtime.  
  
## <a name="see-also"></a>Vedere anche

- [Elemento \<Method>](../../../docs/framework/net-native/method-element-net-native.md)
- [Informazioni di riferimento sul file di configurazione delle direttive di runtime (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)
- [Impostazioni dei criteri delle direttive di runtime](../../../docs/framework/net-native/runtime-directive-policy-settings.md)
- [Elementi direttiva di runtime](../../../docs/framework/net-native/runtime-directive-elements.md)
