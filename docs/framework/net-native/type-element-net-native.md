---
title: <Type> Elemento (.NET Native)
ms.date: 03/30/2017
ms.assetid: 1e88d368-a886-4f1e-8eb6-6127979a9fce
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: fc93d1af65a34381005c3f3987dd18a8e7bba8d9
ms.sourcegitcommit: d8ebe0ee198f5d38387a80ba50f395386779334f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689275"
---
# <a name="type-element-net-native"></a>\<Tipo > elemento (.NET Native)

Applica i criteri di runtime a un determinato tipo, ad esempio una classe o una struttura.

## <a name="syntax"></a>Sintassi

```xml
<Type Name="type_name"
      Activate="policy_type"
      Browse="policy_type"
      Dynamic="policy_type"
      Serialize="policy_type"
      DataContractSerializer="policy_setting"
      DataContractJsonSerializer="policy_setting"
      XmlSerializer="policy_setting"
      MarshalObject="policy_setting"
      MarshalDelegate="policy_setting"
      MarshalStructure="policy_setting" />
```

## <a name="attributes-and-elements"></a>Attributi ed elementi

Nelle sezioni seguenti vengono descritti gli attributi, gli elementi figlio e gli elementi padre.

### <a name="attributes"></a>Attributi

|Attributo|Tipo di attributo|Descrizione|
|---------------|--------------------|-----------------|
|`Name`|Generale|Attributo obbligatorio. Specifica il nome del tipo.|
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
|*type_name*|Nome del tipo. Se questo elemento `<Type>` è figlio di un elemento [\<Namespace>](../../../docs/framework/net-native/namespace-element-net-native.md) o di un altro elemento `<Type>`, *type_name* può includere il nome del tipo senza il relativo spazio dei nomi. In caso contrario, *type_name* deve includere il nome completo del tipo.|

## <a name="all-other-attributes"></a>Tutti gli altri attributi

|Value|Descrizione|
|-----------|-----------------|
|*policy_setting*|L'impostazione da applicare a questo tipo di criteri. I valori consentiti sono `All`, `Auto`, `Excluded`, `Public`, `PublicAndInternal`, `Required Public`, `Required PublicAndInternal` e `Required All`. Per altre informazioni, vedere [Runtime Directive Policy Settings](../../../docs/framework/net-native/runtime-directive-policy-settings.md) (Impostazioni dei criteri delle direttive di runtime).|

### <a name="child-elements"></a>Elementi figlio

|Elemento|Descrizione|
|-------------|-----------------|
|[\<AttributeImplies>](../../../docs/framework/net-native/attributeimplies-element-net-native.md)|Se il tipo contenitore è un attributo, definisce i criteri di runtime per gli elementi del codice a cui è applicato l'attributo.|
|[\<Event>](../../../docs/framework/net-native/event-element-net-native.md)|Applica i criteri di reflection a un evento appartenente a questo tipo.|
|[\<Field>](../../../docs/framework/net-native/field-element-net-native.md)|Applica i criteri di reflection a un campo appartenente a questo tipo.|
|[\<GenericParameter>](../../../docs/framework/net-native/genericparameter-element-net-native.md)|Applica i criteri al tipo di parametro di un tipo generico.|
|[\<ImpliesType>](../../../docs/framework/net-native/impliestype-element-net-native.md)|Applica criteri a un tipo, se tale criterio è stato applicato al tipo rappresentato dall'oggetto contenente l'elemento `<Type>`.|
|[\<Method>](../../../docs/framework/net-native/method-element-net-native.md)|Applica i criteri di reflection a un metodo appartenente a questo tipo.|
|[\<MethodInstantiation>](../../../docs/framework/net-native/methodinstantiation-element-net-native.md)|Applica criteri di reflection a un metodo generico costruito, appartenente a questo tipo.|
|[\<Property>](../../../docs/framework/net-native/property-element-net-native.md)|Applica i criteri di reflection a una proprietà appartenente a questo tipo.|
|[\<Subtypes>](../../../docs/framework/net-native/subtypes-element-net-native.md)|Applica i criteri di runtime a tutte le classi ereditate per il tipo contenitore.|
|`<Type>`|Applica criteri di reflection un tipo annidato.|
|[\<TypeInstantiation>](../../../docs/framework/net-native/typeinstantiation-element-net-native.md)|Applica i criteri di reflection a un tipo generico costruito.|

### <a name="parent-elements"></a>Elementi padre

|Elemento|Descrizione|
|-------------|-----------------|
|[\<Application>](../../../docs/framework/net-native/application-element-net-native.md)|Viene usato come contenitore per i tipi e i membri dei tipi a livello di applicazione i cui metadati sono disponibili per la reflection al runtime.|
|[\<Assembly>](../../../docs/framework/net-native/assembly-element-net-native.md)|Applica i criteri di reflection a tutti i tipi in un determinato assembly.|
|[\<Library>](../../../docs/framework/net-native/library-element-net-native.md)|Definisce l'assembly che contiene i tipi e i membri dei tipi i cui metadati sono disponibili per la reflection al runtime.|
|[\<Namespace>](../../../docs/framework/net-native/namespace-element-net-native.md)|Applica criteri di reflection a tutti i tipi in uno spazio dei nomi.|
|`<Type>`|Applica i criteri di reflection a un tipo e a tutti i membri.|
|[\<TypeInstantiation>](../../../docs/framework/net-native/typeinstantiation-element-net-native.md)|Applica i criteri di reflection a un tipo generico costruito e a tutti i membri.|

## <a name="remarks"></a>Note

La reflection, la serializzazione e gli attributi di interoperabilità sono facoltativi. Se non è presente, l'elemento `<Type>` funge da contenitore i cui tipi figlio definiscono criteri per i singoli membri.

Se un elemento `<Type>` è figlio di un elemento [\<Assembly>](../../../docs/framework/net-native/assembly-element-net-native.md), [\<Namespace>](../../../docs/framework/net-native/namespace-element-net-native.md), `<Type>` o [\<TypeInstantiation>](../../../docs/framework/net-native/typeinstantiation-element-net-native.md), sottopone a override le impostazioni dei criteri definite dall'elemento padre.

Un elemento `<Type>` di un tipo generico applica i relativi criteri a tutte le istanze che non dispongono di propri criteri. I criteri di tipi generici costruiti sono definiti dall'elemento [\<TypeInstantiation>](../../../docs/framework/net-native/typeinstantiation-element-net-native.md).

Se il tipo è un tipo generico, il nome è decorato da un simbolo di accento grave ( \`) seguito dal relativo numero di parametri generici. Ad esempio, l'attributo `Name` di un elemento `<Type>` per la classe <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> viene visualizzato come ``Name="System.Collections.Generic.List`1"``.

## <a name="example"></a>Esempio

Nell'esempio seguente viene usata la reflection per visualizzare le informazioni su campi, proprietà e metodi della classe <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>. La variabile `b` nell'esempio è un <xref:Windows.UI.Xaml.Controls.TextBlock> controllo. Poiché l'esempio recupera semplicemente le informazioni sul tipo, la disponibilità dei metadati è controllata dall'impostazione dei criteri `Browse`.

 [!code-csharp[ProjectN_Reflection#3](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/browsegenerictype1.cs#3)]

 Poiché i metadati per il <xref:System.Collections.Generic.List%601> classe non viene incluso automaticamente dalla catena di strumenti .NET Native, nell'esempio viene eseguito visualizzare le informazioni sul membro richiesto in fase di esecuzione. Per fornire i metadati necessari, aggiungere il seguente elemento `<Type>` al file di direttive di runtime. Da notare che dal momento che è stato fornito un elemento [<Namespace\>](../../../docs/framework/net-native/namespace-element-net-native.md) padre, non è necessario fornire un nome di tipo completo nell'elemento `<Type>`.

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="*Application*" Dynamic="Required All" />
      <Namespace Name ="System.Collections.Generic" >
        <Type Name="List`1" Browse="Required All" />
     </Namespace>
   </Application>
</Directives>
```

## <a name="example"></a>Esempio
 Nell'esempio seguente viene usata la reflection per recuperare un oggetto <xref:System.Reflection.PropertyInfo> che rappresenta la proprietà <xref:System.String.Chars%2A?displayProperty=nameWithType>. Viene quindi usato il metodo <xref:System.Reflection.PropertyInfo.GetValue%28System.Object%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> per recuperare il valore del settimo carattere in una stringa e visualizzare tutti i caratteri nella stringa. La variabile `b` nell'esempio è un <xref:Windows.UI.Xaml.Controls.TextBlock> controllo.

 [!code-csharp[ProjectN_Reflection#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/propertyinfo1.cs#1)]

 Poiché i metadati per il <xref:System.String> oggetto non è disponibile, la chiamata al <xref:System.Reflection.PropertyInfo.GetValue%28System.Object%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> metodo genera un <xref:System.NullReferenceException> eccezione in fase di esecuzione ora quando viene compilato con la catena di strumenti .NET Native. Per eliminare l'eccezione e fornire i metadati necessari, aggiungere il seguente elemento `<Type>` al file di direttive di runtime:

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Assembly Name="*Application*" Dynamic="Required All" />
    <Type Name="System.String" Dynamic="Required Public"/> -->
  </Application>
</Directives>
```

## <a name="see-also"></a>Vedere anche

- [Informazioni di riferimento sul file di configurazione delle direttive di runtime (rd.xml)](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)
- [Elementi direttiva di runtime](../../../docs/framework/net-native/runtime-directive-elements.md)
- [Impostazioni dei criteri delle direttive di runtime](../../../docs/framework/net-native/runtime-directive-policy-settings.md)
