---
title: Cronologia di C# - Guida a C#
description: Caratteristiche del linguaggio nelle prime versioni ed evoluzione successiva.
author: erikdietrich
ms.date: 09/20/2017
ms.openlocfilehash: 351e4de01a7c3c50cb815a99f0bde546848a9e51
ms.sourcegitcommit: 10986410e59ff29f2ec55c6759bde3eb4d1a00cb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66423245"
---
# <a name="the-history-of-c"></a>Cronologia di C\#

Questo articolo include la cronologia di ogni versione principale del linguaggio C#. Il team di C# continua a innovare il prodotto e ad aggiungere nuove funzionalità. Informazioni dettagliate sullo stato delle funzionalità del linguaggio, include le funzionalità prese in considerazione per versioni future, sono reperibili [nel repository dotnet/roslyn](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md) su GitHub.

> [!IMPORTANT]
> Per alcune funzionalità, il linguaggio C# si basa sui tipi e metodi inclusi in quella che la specifica C# definisce la *libreria standard*. La piattaforma .NET rende disponibili questi tipi e metodi in numerosi pacchetti. Un esempio è l'elaborazione delle eccezioni. Ogni istruzione o espressione `throw` viene controllata per assicurarsi che l'oggetto generato derivi da <xref:System.Exception>. Analogamente, ogni istruzione `catch` viene controllata per verificare che il tipo intercettato derivi da <xref:System.Exception>. In ogni versione potrebbero essere aggiunti nuovi requisiti. Per usare le funzionalità del linguaggio più recenti in ambienti meno recenti, potrebbe essere necessario installare librerie specifiche. Queste dipendenze sono documentate nella pagina per ogni versione specifica. Le informazioni sulle [relazioni tra linguaggio e libreria](relationships-between-language-and-library.md) possono essere utili per comprendere meglio questa dipendenza.

Gli strumenti di compilazione per C# considerano la versione del linguaggio principale più recente la versione del linguaggio predefinita. Potrebbero esistere versioni intermedie tra le versioni principali, descritte in dettaglio in altri articoli in questa sezione. Per usare le funzionalità più recenti in una versione intermedia, è necessario [configurare la versione in lingua del compilatore](../language-reference/configure-language-version.md) e selezionare la versione. Sono state rilasciate tre versioni intermedie da C# 7.0:

* [C# 7.3](csharp-7-3.md):
  - C# 7.3 è disponibile a partire da [Visual Studio 2017 versione 15.7](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) e [.NET Core 2.1 SDK](../../core/whats-new/dotnet-core-2-1.md).
* [C# 7.2](csharp-7-2.md):
  - C# 7.2 è disponibile a partire da [Visual Studio 2017 versione 15.5](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) e [.NET Core 2.0 SDK](../../core/whats-new/dotnet-core-2-0.md).
* [C# 7.1](csharp-7-1.md):
  - C# 7.1 è disponibile a partire da [Visual Studio 2017 versione 15.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) e [.NET Core 2.0 SDK](../../core/whats-new/dotnet-core-2-0.md).

## <a name="c-version-10"></a>C# versione 1.0

Se ci si guarda indietro, ci si rende conto che C# versione 1.0 era molto simile a Java. Come affermato [negli obiettivi di progettazione dichiarati per ECMA](https://feeldotneteasy.blogspot.com/2011/01/c-design-goals.html), C# cercava di essere "un linguaggio orientato agli oggetti di utilizzo generico, semplice e moderno".  All'epoca, somigliare molto a Java significava aver raggiunto tali obiettivi di progettazione di allora.

Ma se si guarda ora com'era C# 1.0, la sensazione è di disorientamento. Mancavano le capacità asincrone predefinite e alcune delle semplici funzionalità relative ai generics che ora si danno per scontate. In realtà,i generics mancavano completamente.  E [LINQ](../linq/index.md)? Non era ancora disponibile. Queste aggiunte avrebbero richiesto ancora alcuni anni.

Rispetto alla versione odierna, C# versione 1.0 sembra privo di funzionalità e costringeva gli sviluppatori a scrivere codice piuttosto prolisso. Da qualche parte, tuttavia, bisognava cominciare. Per la piattaforma Windows, C# versione 1.0 rappresentava una valida alternativa a Java.

Le principali funzionalità di C# 1.0 includevano:

- [Classi](../programming-guide/classes-and-structs/classes.md)
- [Struct](../programming-guide/classes-and-structs/structs.md)
- [Interfacce](../programming-guide/interfaces/index.md)
- [Eventi](../events-overview.md)
- [Proprietà](../properties.md)
- [Delegati](../delegates-overview.md)
- [Espressioni](../programming-guide/statements-expressions-operators/expressions.md)
- [Istruzioni](../programming-guide/statements-expressions-operators/statements.md)
- [Attributi](../programming-guide/concepts/attributes/index.md)

## <a name="c-version-12"></a>Versione C# 1.2

Versione C# 1.2 fornita con Visual Studio 2003. Conteneva alcuni piccoli miglioramenti del linguaggio. L'aspetto più importante è che a partire da questa versione, il codice veniva generato in un ciclo `foreach` denominato <xref:System.IDisposable.Dispose%2A> in <xref:System.Collections.IEnumerator> quando <xref:System.Collections.IEnumerator> implementava <xref:System.IDisposable>.

## <a name="c-version-20"></a>C# versione 2.0

Qui le cose iniziano a farsi interessanti. Ecco alcune delle funzionalità principali di C# 2.0, rilasciato nel 2005 insieme a Visual Studio 2005:

- [Generics](../programming-guide/generics/index.md)
- [Tipi parziali](../programming-guide/classes-and-structs/partial-classes-and-methods.md#partial-classes)
- [Metodi anonimi](../programming-guide/statements-expressions-operators/anonymous-methods.md)
- [Tipi nullable](../programming-guide/nullable-types/index.md)
- [Iteratori](../programming-guide/concepts/iterators.md)
- [Covarianza e controvarianza](../programming-guide/concepts/covariance-contravariance/index.md)

Altre funzionalità di C# 2.0 aggiungevano capacità alle funzionalità esistenti:

- Accessibilità separata getter/setter
- Conversioni di gruppi di metodi (delegati)
- Classi statiche
- Inferenza del delegato

All'inizio C# era un linguaggio orientato agli oggetti (OO) generico, ma con la versione 2.0 la situazione cambiò con grande rapidità. Dopo aver trovato una posizione stabile, gli sviluppatori di C# hanno affrontato alcuni gravi punti dolenti. E li hanno affrontati alla grande.

Con generics, tipi e metodi possono operare su un tipo arbitrario, mantenendo comunque l'indipendenza dal tipo. La classe <xref:System.Collections.Generic.List%601>, ad esempio, consente di eseguire un'iterazione attraverso `List<string>` o `List<int>` eseguendo operazioni indipendenti dai tipi su tali stringhe o tali numeri interi. È consigliabile usare i generics anziché creare `ListInt` che deriva da `ArrayList` o eseguire il cast da `Object` per ogni operazione.

Con C# versione 2.0 sono arrivati gli iteratori. In breve, gli iteratori consentono di esaminare tutti gli elementi di un `List` (o di altri tipi enumerabili) con un ciclo `foreach`. La presenza degli iteratori come parte fondamentale del linguaggio ha migliorato notevolmente la leggibilità del codice e la possibilità di comprenderlo.

C#, tuttavia, continuava a correre dietro a Java, che aveva già rilasciato versioni che prevedevano generics e iteratori, ma questa situazione sarebbe cambiata presto. Man mano che procedevano nella loro evoluzione, infatti, i due linguaggi continuarono a differenziarsi.

## <a name="c-version-30"></a>C# versione 3.0

C# versione 3.0 è stato rilasciato alla fine del 2007, insieme a Visual Studio 2008, anche se la gamma completa delle funzionalità del linguaggio è stata in realtà rilasciata con .NET Framework versione 3.5. Questa versione rappresenta un cambiamento fondamentale, perché caratterizza C# come linguaggio di programmazione dalle caratteristiche davvero eccezionali. Ecco alcune delle funzionalità principali di questa versione:

- [Proprietà implementate automaticamente](../programming-guide/classes-and-structs/auto-implemented-properties.md)
- [Tipi anonimi](../programming-guide/classes-and-structs/anonymous-types.md)
- [Espressioni di query](../linq/query-expression-basics.md)
- [Espressioni lambda](../programming-guide/statements-expressions-operators/lambda-expressions.md)
- [Alberi delle espressioni](../expression-trees.md)
- [Metodi di estensione](../programming-guide/classes-and-structs/extension-methods.md)
- [Variabili locali tipizzate in modo implicito](../language-reference/keywords/var.md)
- [Metodi parziali](../language-reference/keywords/partial-method.md)
- [Inizializzatori di oggetto e di insieme](../programming-guide/classes-and-structs/object-and-collection-initializers.md)

A posteriori, molte di queste funzionalità sembrano inevitabili e inseparabili, perché si fondono l'una con l'altra in modo strategico. Si pensa in genere che la funzionalità killer di questa versione di C# sia l'espressione di query, nota anche come Language-Integrated Query (LINQ),

che, grazie a una vista più particolareggiata, consente di esaminare alberi delle espressioni, espressioni lambda e tipi anonimi, le fondamenta su cui la funzionalità LINQ stessa si basa. In entrambi i casi, tuttavia, C# 3.0 ha introdotto un concetto rivoluzionario. C# 3.0 ha iniziato a creare i presupposti per la trasformazione di C# in un linguaggio ibrido, orientato agli oggetti e funzionale.

In particolare, ha reso possibile scrivere query dichiarative nello stile di SQL per eseguire operazioni, tra l'altro, sulle raccolte. Anziché scrivere un ciclo `for` per calcolare la media di un elenco di numeri interi, è diventato possibile eseguire tale operazione semplicemente usando `list.Average()`. La combinazione di espressioni di query e di metodi di estensione rendeva quell'elenco di numeri interi molto più interessante.

La comprensione e l'integrazione effettive del concetto da parte degli utenti ha richiesto tempo, ma si è gradualmente realizzata. E ora, dopo anni, il codice è molto più conciso, semplice e funzionale.

## <a name="c-version-40"></a>C# versione 4.0

Per C# versione 4.0 è stato difficile essere all'altezza della rivoluzionaria versione 3.0. Con la versione 3.0, il linguaggio C# è uscito con decisione dall'ombra di Java e ha assunto una propria rilevanza. Il linguaggio stava rapidamente diventando elegante.

La versione successiva introdusse comunque alcune nuove interessanti funzionalità:

- [Associazione dinamica](../language-reference/keywords/dynamic.md)
- [Argomenti denominati/facoltativi](../programming-guide/classes-and-structs/named-and-optional-arguments.md)
- [Covarianti e controvarianti generiche](../../standard/generics/covariance-and-contravariance.md)
- [Tipi di interoperabilità incorporati](../../framework/interop/type-equivalence-and-embedded-interop-types.md)

I tipi di interoperabilità risolvevano un problema di distribuzione. Le covarianze e le controvarianze generiche offrono modalità più avanzate per l'uso dei generics, ma hanno un'aria accademica e sono probabilmente apprezzate soprattutto dagli autori di librerie e di framework. I parametri denominati e facoltativi consentono di eliminare molti overload di metodi e sono più comodi da usare. Ma nessuna di queste funzionalità è precisamente un modifica di paradigma.

La caratteristica principale è stata l'introduzione della parola chiave `dynamic`. La parola chiave `dynamic` introdotta in C# versione 4.0 offre la possibilità di eseguire l'override del compilatore per la tipizzazione in fase di compilazione. Con la parola chiave dynamic è possibile creare costrutti simili a quelli dei linguaggi tipizzati in modo dinamico, come JavaScript. È possibile creare `dynamic x = "a string"` e quindi aggiungere 6, lasciando decidere al runtime cosa deve succedere dopo.

L'associazione dinamica è suscettibile di errori ma offre anche la possibilità di usare il linguaggio in modo estremamente avanzato.

## <a name="c-version-50"></a>C# versione 5.0

C# versione 5.0 è una versione incentrata sul linguaggio. Quasi tutto l'impegno per questa versione è stato indirizzato verso un altro concetto rivoluzionario: il modello `async` e `await` per la programmazione asincrona.  Ecco l'elenco delle funzionalità principali:

- [Membri asincroni](../async.md)
- [Attributi informativi sul chiamante](../programming-guide/concepts/caller-information.md)

### <a name="see-also"></a>Vedere anche

* [Code Project: Caller Info Attributes in C# 5.0](https://www.codeproject.com/Tips/606379/Caller-Info-Attributes-in-Csharp) (Attributi informativi sul chiamante in C# 5.0)

Gli attributi informativi sul chiamante consentono di recuperare facilmente informazioni sul contesto di esecuzione senza dover ricorrere a una grande quantità di codice di reflection boilerplate. Questi attributi hanno molte applicazioni nelle attività di diagnostica e di registrazione.

Ma le vere stelle di questa versione sono `async` e `await`. Con il rilascio di queste funzionalità nel 2012, C# ha cambiato di nuovo le carte in tavola, incorporando l'asincronia nel linguaggio e assegnandole un ruolo fondamentale. Tutti coloro che hanno avuto a che fare con operazioni di lunga esecuzione e con l'implementazione di reti di callback hanno molto apprezzato questa funzionalità.

## <a name="c-version-60"></a>C# versione 6.0

Le versioni 3.0 e 5.0 hanno aggiunto a C# nuove funzionalità eccezionali per un linguaggio orientato agli oggetti. Con la versione 6.0, anziché una funzionalità killer che attirasse tutta l'attenzione, sono state rilasciate molte funzionalità che hanno reso più produttiva la programmazione in C#. Eccone alcune:

- [Importazioni statiche](./csharp-6.md#using-static)
- [Filtri eccezioni](./csharp-6.md#exception-filters)
- [Inizializzatori di proprietà automatiche](./csharp-6.md#auto-property-initializers)
- [Membri con corpo di espressione](./csharp-6.md#expression-bodied-function-members)
- [Propagazione Null](./csharp-6.md#null-conditional-operators)
- [Interpolazione di stringhe](./csharp-6.md#string-interpolation)
- [Operatore nameof](./csharp-6.md#the-nameof-expression)
- [Inizializzatori di indice](csharp-6.md#extension-add-methods-in-collection-initializers)

Le altre nuove funzionalità includono:

- Await nei blocchi catch e finally
- Valori predefiniti per le proprietà solo getter

Ognuna di queste funzionalità è interessante in sé, ma osservandole nel loro complesso si scopre uno schema interessante. In questa versione, C# ha eliminato il boilerplate del linguaggio, per rendere il codice più conciso e leggibile. Per gli amanti della pulizia e della semplicità del codice, questa versione del linguaggio rappresenta una vittoria notevole.

Con questa versione è stata introdotta un'altra caratteristica, che di per sé non rappresenta una funzionalità tradizionale del linguaggio. È stato rilasciato [Roslyn, il compilatore come servizio](https://github.com/dotnet/roslyn). Il compilatore C# è ora scritto in C# e può essere usato all'interno dei progetti di programmazione.

## <a name="c-version-70"></a>C# versione 7.0

La versione principale più recente di C# è la 7.0. Questa versione presenta alcune delle caratteristiche evolutive e innovative di C# 6.0, ma senza il compilatore come servizio. Ecco alcune delle nuove funzionalità:

- [Variabili out](./csharp-7.md#out-variables)
- [Tuple e decostruzione](./csharp-7.md#tuples)
- [Criteri di ricerca](./csharp-7.md#pattern-matching)
- [Funzioni locali](./csharp-7.md#local-functions)
- [Membri di espressioni corpo espansi](./csharp-7.md#more-expression-bodied-members)
- [Variabili locali e valori restituiti per riferimento](./csharp-7.md#ref-locals-and-returns)

Altre funzionalità:

- [Variabili discard](./csharp-7.md#discards)
- [Valori letterali binari e separatori di cifre](./csharp-7.md#numeric-literal-syntax-improvements)
- [Espressioni throw](./csharp-7.md#throw-expressions)

Tutte queste caratteristiche offrono nuove utili funzionalità agli sviluppatori, oltre alla possibilità di scrivere codice più pulito che mai. Una funzionalità di particolare rilievo è la possibilità di condensare la dichiarazione di variabili da usare con la parola chiave `out`, consentendo più valori restituiti tramite tupla.

Ma C# è ora destinato a un uso ancora più ampio. .NET Core ora supporta qualsiasi sistema operativo ed è decisamente orientato al cloud e alla portabilità.  Queste nuove capacità, insieme alla realizzazione di nuove funzionalità, tengono sicuramente impegnati i progettisti del linguaggio.

_Articolo_ [ _originariamente pubblicato nel blog NDepend_](https://blog.ndepend.com/c-versions-look-language-history/) _, gentilmente concesso da Erik Dietrich e Patrick Smacchia._
