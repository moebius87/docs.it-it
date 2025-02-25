---
title: Mapping tra JSON e XML
ms.date: 03/30/2017
ms.assetid: 22ee1f52-c708-4024-bbf0-572e0dae64af
ms.openlocfilehash: ef5eaac8fc75149ac518ce322808a84bbab5506b
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65636429"
---
# <a name="mapping-between-json-and-xml"></a>Mapping tra JSON e XML
I lettori e i writer prodotti da <xref:System.Runtime.Serialization.Json.JsonReaderWriterFactory> forniscono una API XML sul contenuto JSON (JavaScript Object Notation). JSON codifica i dati utilizzando un sottoinsieme dei valori letterali di oggetto di JavaScript. I lettori e i writer prodotti da questa factory vengono usati anche quando il contenuto JSON viene inviato o ricevuto da applicazioni di Windows Communication Foundation (WCF) usando il <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement> o il <xref:System.ServiceModel.WebHttpBinding>.

In caso di inizializzazione con contenuto JSON, il lettore JSON si comporta nello stesso modo di un lettore XML testuale su un'istanza di XML. Il writer JSON, in presenza di una sequenza di chiamate prodotte da una certa istanza XML su un lettore XML testuale, scrive il contenuto JSON. In questo argomento viene illustrato il mapping tra questa istanza di XML e il contenuto JSON, per l'utilizzo in scenari avanzati.

Internamente, JSON è rappresentato come un infoset XML quando elaborato da WCF. In genere non è necessario occuparsi di questa rappresentazione interna poiché il mapping è solo logico: JSON è in genere non è fisicamente convertito in XML in memoria o convertito in JSON da XML. Il mapping significa che le API XML sono utilizzate per accedere a contenuto JSON.

Quando WCF utilizza JSON, di norma è che il <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> viene automaticamente inserito dal <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> comportamento, o tramite il <xref:System.ServiceModel.Description.WebHttpBehavior> comportamento quando appropriato. <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> è in grado di comprendere il mapping tra JSON e l'infoset XML e si comporta come se stesse interagendo direttamente con JSON. È possibile utilizzare <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> con qualsiasi lettore o writer XML, a condizione che XML sia conforme al mapping seguente.

Negli scenari avanzati, può rendersi necessario accedere direttamente al mapping seguente. Questi scenari si verificano quando si desidera serializzare e deserializzare JSON in modalità personalizzate, senza basarsi su <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>o quando si interagisce direttamente con il tipo <xref:System.ServiceModel.Channels.Message> per messaggi che contengono JSON. Il mapping JSON-XML è utilizzato anche per la registrazione dei messaggi. Quando si usa la funzionalità di registrazione dei messaggi in WCF, i messaggi JSON vengono registrati come XML in base al mapping descritto nella sezione successiva.

Per chiarire il concetto di mapping, l'esempio seguente fa riferimento a un documento JSON.

```json
{"product":"pencil","price":12}
```

Per leggere il documento JSON tramite uno dei lettori indicato in precedenza, utilizzare la stessa sequenza di chiamate a <xref:System.Xml.XmlDictionaryReader> utilizzata per leggere il documento XML seguente.

```xml
<root type="object">
    <product type="string">pencil</product>
    <price type="number">12</price>
</root>
```

Inoltre, se il messaggio JSON nell'esempio viene ricevuto da WCF e registrato, si vedrebbe il frammento XML nel log precedente.

## <a name="mapping-between-json-and-the-xml-infoset"></a>Mapping tra JSON e l'InfoSet XML
Formalmente, il mapping è tra JSON come descritto in [RFC 4627](https://go.microsoft.com/fwlink/?LinkId=98808) (tranne che con alcune restrizioni ridotte e altre restrizioni aggiunte) e il XML infoset (e XML non testuale) come descritto in [informazioni XML Impostare](https://go.microsoft.com/fwlink/?LinkId=98809) . Per le definizioni di vedere l'argomento *voci di informazioni* e i campi in [parentesi quadre].

Un documento JSON vuoto esegue il mapping a un documento XML vuoto e un documento XML vuoto esegue il mapping a un documento JSON vuoto. Nel mapping da XML a JSON, che precede lo spazio vuoto e lo spazio vuoto finale dopo il documento non sono consentiti.

Il mapping viene definito tra un DII (Document Information Item) o un EII (Element Information Item) e JSON. La proprietà [elemento del documento] di EII o DII, viene chiamata elemento JSON radice. Si noti che in questo mapping non sono supportati frammenti di documento (XML con più elementi radice).

Esempio: Il documento seguente:

```xml
<?xml version="1.0"?>
<root type="number">42</root>`
```

E l'elemento seguente:

```xml
<root type="number">42</root>
```

Entrambi hanno un mapping a JSON. Il <`root`> è l'elemento JSON radice in entrambi i casi.

Nel caso di un DII, inoltre, è necessario prendere in considerazione gli elementi seguenti:

- Alcuni elementi nell'elenco [figli] non devono essere presenti. Non fare affidamento su tale situazione in caso di lettura di XML mappato da JSON.

- L'elenco [figli] non contiene voci di informazioni di commento.

- L'elenco [figli] non contiene voci di informazioni DTD.

- L'elenco [figli] non contiene nessun voci di informazioni personali di informazioni (PI) (le `<?xml…>` dichiarazione non è considerata una voce PI)

- Il set [notazioni] è vuoto.

- Il set [entità non analizzate] è vuoto.

Esempio: Il documento seguente è contenuto alcun mapping in formato JSON perché [figli] contiene una PI e un commento.

```xml
<?xml version="1.0"?>
<!--comment--><?pi?>
<root type="number">42</root>
```

L'EII per l'Elemento JSON radice ha le caratteristiche seguenti:

- [nome locale] ha il valore "root".

- [nome dello spazio dei nomi] non ha valore.

- [prefisso] non ha valore.

- [figli] può contenere EII (che rappresentano elementi interni come descritto più avanti) o CII (Character Information Items come descritto più avanti) o nessuno dei due, ma non entrambi.

- [attributi] può contenere le voci di informazioni sugli attributi facoltativi seguenti (AII)

- L'attributo di tipo JSON ("type") come descritto più avanti. Questo attributo è utilizzato per mantenere il tipo JSON (stringa, numero, booleano, oggetto, matrice o null) nell'XML di cui è stato eseguito il mapping.

- L'attributo del nome di contratto dati ("\_\_tipo") come descritto più avanti. Questo attributo può essere presente solo se è presente anche l'attributo del tipo JSON e il suo [valore normalizzato] è "object". Questo attributo è utilizzato da `DataContractJsonSerializer` per mantenere informazioni sul tipo di contratto dati , ad esempio, nei casi polimorfici in cui viene serializzato un tipo derivato ed è previsto un tipo di base. Se non si utilizza `DataContractJsonSerializer`, nella maggior parte dei casi questo attributo viene ignorato.

- [spazi dei nomi nell'ambito] contiene l'associazione di "xml" per `http://www.w3.org/XML/1998/namespace` come indicato dalla specifica infoset.

- [figli], [attributi] e [spazi dei nomi nell'ambito] non devono avere altri elementi tranne quelli specificati in precedenza e [attributi dello spazio dei nomi] non deve avere membri, ma non fare affidamento su questa situazione in caso di lettura di XML mappato da JSON.

Esempio: Il documento seguente è contenuto alcun mapping in formato JSON perché [attributi dello spazio dei nomi] non è vuota.

```xml
<?xml version="1.0"?>
<root xmlns:a="myattributevalue">42</root>
```

L'AII per l'attributo del tipo JSON ha le caratteristiche seguenti:

- [nome dello spazio dei nomi] non ha valore.
- [prefisso] non ha valore.
- [nome locale] è "type".
- [valore normalizzato] è uno dei possibili valori del tipo descritto nella sezione seguente.
- [specificato] è `true`.
- [tipo attributo] non ha valore.
- [riferimenti] non ha valore.

L'AII per l'attributo del nome del contratto dati ha le caratteristiche seguenti:

- [nome dello spazio dei nomi] non ha valore.
- [prefisso] non ha valore.
- [nome locale] è "\_\_tipo" (due caratteri di sottolineatura e quindi "type").
- [valore normalizzato] è qualsiasi stringa Unicode valida, il mapping di questa stringa a JSON viene descritto nella sezione seguente.
- [specificato] è `true`.
- [tipo attributo] non ha valore.
- [riferimenti] non ha valore.

Gli elementi interni contenuti all'interno dell'Elemento JSON radice o altri elementi interni hanno le caratteristiche seguenti:

- [nome locale] può avere qualsiasi valore, come descritto più avanti.
- [nome dello spazio dei nomi], [prefisso], [figli], [attributi], [attributi dello spazio dei nomi] e [spazi dei nomi nell'ambito] sono soggetti alle stesse regole dell'Elemento JSON radice.

Sia nell'Elemento JSON radice che negli elementi interni, l'attributo del tipo JSON definisce il mapping a JSON, i [figli] possibili e la loro interpretazione. [Valore normalizzato] dell'attributo tra maiuscole e minuscole e deve essere minuscolo e non può contenere spazi vuoti.

|[valore normalizzato] dell'AII dell'attributo di tipo JSON|[figli] consentiti dell'EII corrispondente|Mapping a JSON|
|---------------------------------------------------------|---------------------------------------------------|---------------------|
|`string` (o assenza dell'AII del tipo JSON)<br /><br /> Una `string` e l'assenza dell'AII del tipo JSON sono la stessa cosa e creano l'elemento predefinito `string`.<br /><br /> Pertanto, `<root> string1</root>``string` esegue il mapping alla  "string1" JSON.|0 o più Cii|Oggetto `string` JSON (JSON RFC, sezione 2.5). Ogni `char` è un carattere che corrisponde al [character code] di CII. Se non sono presenti CII, esegue il mapping a una `string` JSON vuota.<br /><br /> Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON:<br /><br /> `<root type="string">42</root>`<br /><br /> Il frammento JSON è "42".<br /><br /> Nel mapping da XML a JSON, i caratteri che devono essere preceduti dal carattere di escape vengono mappati ai caratteri di escape, tutti gli altri vengono mappati a caratteri non di escape. Il carattere "/" è speciale – viene sottoposto a escape anche se non dovrà essere (scritto come "\\/").<br /><br /> Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.<br /><br /> `<root type="string">the "da/ta"</root>`<br /><br /> Il frammento JSON è "il \\" da\\/ta\\"".<br /><br /> Nel mapping da JSON a XML, qualsiasi carattere preceduto o no da un carattere di escape viene mappato correttamente al [codice carattere] corrispondente.<br /><br /> Esempio: Il frammento JSON "\u0041BC", viene eseguito il mapping all'elemento XML seguente.<br /><br /> `<root type="string">ABC</root>`<br /><br /> La stringa può essere precedute e seguita da spazi vuoti ('ws' nella sezione 2 di JSON RFC) che non vengono mappati a XML.<br /><br /> Esempio: Il codice JSON frammento "ABC", (sono presenti spazi prima le doppie virgolette di apertura), viene eseguito il mapping all'elemento XML seguente.<br /><br /> `<root type="string">ABC</root>`<br /><br /> Gli spazi vuoti nel file XML viene mappato a spazio vuoto in JSON.<br /><br /> Esempio: L'elemento XML seguente viene eseguito il mapping a un frammento JSON.<br /><br /> `<root type="string">  A BC      </root>`<br /><br /> Il frammento JSON è " A BC ".|
|`number`|1 o più CII|Un file JSON `number` (JSON RFC, sezione 2.4), eventualmente racchiuso da spazio vuoto. Ogni carattere nella combinazione numero/spazio è un carattere che corrisponde al [codice carattere] di CII.<br /><br /> Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.<br /><br /> `<root type="number">    42</root>`<br /><br /> Il frammento JSON è    42<br /><br /> (Lo spazio vuoto viene mantenuto).|
|`boolean`|4 o 5 Cii (che corrisponde a `true` o `false`), possibilmente circondati da Cii gli spazi vuoti aggiuntivi.|Una sequenza di CII che corrisponde alla stringa "true" viene mappata al valore letterale `true` e la sequenza di CII che corrisponde alla stringa "false" viene mappata al valore letterale `false`. Lo spazio vuoto circostante viene mantenuto.<br /><br /> Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.<br /><br /> `<root type="boolean"> false</root>`<br /><br /> Il frammento JSON è `false`.|
|`null`|Nessuno consentito.|Il valore letterale `null`. In JSON per il mapping di XML, il `null` possono essere precedute e seguite da spazi vuoti ('ws' nella sezione 2) che non vengono mappati a XML.<br /><br /> Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.<br /><br /> `<root type="null"/>`<br /><br /> oppure<br /><br /> `<root type="null"></root>`<br /><br /> :<br /><br /> In entrambi i casi il frammento JSON è `Null`.|
|`object`|0 o più EII.|`begin-object` (parentesi graffa aperta) come nella sezione 2.2 di JSON RFC, seguito da un record del membro per ogni EII come descritto più avanti. Se esiste più di un EII, sono presenti separatori di valore (virgole) tra il record dei membri. Tutto è seguito da un fine oggetto (parentesi graffa chiusa).<br /><br /> Esempio: L'elemento seguente viene mappato al frammento JSON.<br /><br /> `<root type="object">`<br /><br /> `<type1 type="string">aaa\</type1>`<br /><br /> `<type2 type="string">bbb\</type2>`<br /><br /> `</root >`<br /><br /> Il frammento JSON è `{"type1":"aaa","type2":"bbb"}`.<br /><br /> Se nel mapping da XML a JSON è presente l'attributo tipo di contratto dati, all'inizio viene inserito un record del membro aggiuntivo. Il nome è il [nome locale] dell'attributo del tipo di contratto dati ("\_\_tipo"), e il relativo valore è l'attributo [valore normalizzato]. Viceversa, in JSON per il mapping di XML, se nome del primo record del membro è il [nome locale] dell'attributo del tipo di contratto dati (vale a dire, "\_\_tipo"), un attributo di tipo di contratto dati corrispondente è presente nel XML Schema mappato, ma un EII corrispondente non è presente. Si noti che, perché questo mapping speciale si applichi, il record del membro deve essere per primo nell'oggetto JSON. Ciò si discosta dall'elaborazione JSON abituale, in cui l'ordine dei record dei membri non è importante.<br /><br /> Esempio:<br /><br /> il frammento JSON seguente viene mappato a XML.<br /><br /> `{"__type":"Person","name":"John"}`<br /><br /> L'XML è il codice seguente.<br /><br /> `<root type="object" __type="Person">   <name type="string">John</name> </root>`<br /><br /> Si noti che il \_ \_tipo AII è presente, ma è presente alcun \_ \_digitare EII.<br /><br /> Se, tuttavia, l'ordine in JSON è invertito come illustrato nell'esempio seguente.<br /><br /> {"name":"John","\_\_type":"Person"}<br /><br /> Viene illustrato l'XML corrispondente.<br /><br /> `<root type="object">   <name type="string">John</name>   <__type type="string">Person</__type> </root>`<br /><br /> Vale a dire, \_Type cessa di avere un significato speciale e viene mappato a un EII come di consueto, non ad AII.<br /><br /> Le regole sull'utilizzo o meno di caratteri di escape per il [valore normalizzato] di AII quando mappato a un valore JSON sono identiche a quelle per le stringhe JSON, come specificato nella riga "string" di questa tabella.<br /><br /> Esempio:<br /><br /> `<root type="object" __type="\abc" />`<br /><br /> all'esempio precedente può essere mappato al JSON seguente.<br /><br /> `{"__type":"\\abc"}`<br /><br /> In un mapping da XML a JSON, non deve essere il primo EII [nome locale] "\_\_tipo".<br /><br /> Gli spazi vuoti (`ws`) non viene mai generato su XML per il mapping a JSON per gli oggetti e viene ignorato per il mapping XML in JSON.<br /><br /> Esempio: Il frammento JSON seguente viene eseguito il mapping a un elemento XML.<br /><br /> `{ "ccc" : "aaa", "ddd" :"bbb"}`<br /><br /> Nel codice seguente viene illustrato l'elemento XML.<br /><br /> `<root type="object">    <ccc type="string">aaa</ccc>    <ddd type="string">bbb</bar> </root >`|
|array|0 o più EII|Un inizio di matrice (parentesi quadra aperta) come nella sezione 2.3 di JSON RFC, seguita da un record della matrice per ogni EII come descritto più avanti. Se esiste più di un EII, sono presenti separatori di valore (virgole) tra i record delle matrici. Il tutto, è seguito da un fine matrice.<br /><br /> Esempio: L'elemento XML seguente viene eseguito il mapping a un frammento JSON.<br /><br /> `<root type="array"/>    <item type="string">aaa</item>    <item type="string">bbb</item> </root >`<br /><br /> Il frammento JSON è ["aaa","bbb"]<br /><br /> Gli spazi vuoti (`ws`) non viene mai generato su XML per il mapping a JSON per le matrici e viene ignorato per il mapping XML in JSON.<br /><br /> Esempio: Un frammento JSON.<br /><br />`["aaa", "bbb"]`<br /><br /> L'elemento XML su cui viene eseguito il mapping.<br /><br /> `<root type="array"/>    <item type="string">aaa</item>    <item type="string">bbb</item> </root >`|

I record dei membri funzionano come segue:

- Il [nome locale] dell'elemento interno viene mappato alla parte `string` di `member`, come definito nella sezione 2.2 di JSON RFC.

Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.

`<root type="object"/>`

`<myLocalName type="string">aaa</myLocalName>`

`</root >`

Viene visualizzato il frammento JSON seguente.

`{"myLocalName":"aaa"}`

- Nel mapping da XML a JSON, i caratteri che devono essere preceduti dal carattere di escape in JSON sono preceduti dal carattere di escape, gli altri no. Il carattere "/",viene preceduto da un carattere di escape anche se ciò non è necessario (non richiede il carattere di escape nel mapping da JSON a XML). Ciò è necessario per supportare il formato ASP.NET AJAX per i dati `DateTime` in JSON.

- Nel mapping da JSON a XML, vengono presi tutti i caratteri (compresi quelli senza carattere di escape, se necessario) per formare una `string` che produce un [nome locale].

- I [figli] dell'elemento interno vengono mappati al valore nella sezione 2.2, secondo `JSON Type Attribute` come per `Root JSON Element`. Sono consentiti più livelli di annidamento di EII (incluso l'annidamento all'interno di matrici).

Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.

```xml
<root type="object">
    <myLocalName1 type="string">myValue1</myLocalName1>
    <myLocalName2 type="number">2</myLocalName2>
    <myLocalName3 type="object">
        <myNestedName1 type="boolean">true</myNestedName1>
        <myNestedName2 type="null"/>
    </myLocalName3>
</root >
```

Viene mappato al frammento JSON seguente.

`{"myLocalName1":"myValue1","myLocalName2":2,"myLocalName3":{"myNestedName1":true,"myNestedName2":null}}`

> [!NOTE]
> Nel mapping precedente non esiste alcun passaggio di codifica XML. Di conseguenza, WCF supporta solo i documenti JSON in cui tutti i caratteri nei nomi delle chiavi sono caratteri validi nei nomi di elemento XML. Ad esempio, il documento JSON {"<": "a"} non è supportato perché < non è un nome valido per un elemento XML.

La situazione inversa (caratteri validi in XML ma non in JSON) non causa alcun problema perché il mapping precedente include passaggi con/senza caratteri di escape in JSON.

I record di matrici funzionano come segue:

- Il [nome locale] dell'elemento interno è "item".

- I [figli] dell'elemento interno vengono mappati al valore nella sezione 2.3, secondo l'attributo Type di JSON così come per l'elemento JSON radice. Sono consentiti più livelli di annidamento di EII (incluso l'annidamento all'interno di oggetti).

Esempio: L'elemento seguente viene eseguito il mapping a un frammento JSON.

```xml
<root type="array"/>
    <item type="string">myValue1</item>
    <item type="number">2</item>
    <item type="array">
    <item type="boolean">true</item>
    <item type="null"/></item>
</root >
```

Quello che segue è il frammento JSON.

`["myValue1",2,[true,null]]`

## <a name="see-also"></a>Vedere anche

- <xref:System.Runtime.Serialization.Json.JsonReaderWriterFactory>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>
- [Serializzazione JSON autonoma](stand-alone-json-serialization.md)
