---
title: Tipi di base - Guida a C#
description: Informazioni sui tipi di base (dati numerici, stringhe e oggetto) in tutti i programmi C#
ms.date: 10/10/2016
ms.assetid: 95c686ba-ae4f-440e-8e94-0dbd6e04d11f
ms.openlocfilehash: 3619e1dc9a82c7f120680c198c327252744444b4
ms.sourcegitcommit: 10986410e59ff29f2ec55c6759bde3eb4d1a00cb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66422093"
---
# <a name="types-variables-and-values"></a>Tipi, variabili e valori

C# è un linguaggio fortemente tipizzato. Ogni variabile e costante ha un tipo, così come ogni espressione che restituisce un valore. Ogni firma del metodo specifica un tipo per ogni parametro di input e per il valore restituito. La libreria di classi .NET Framework definisce un set di tipi numerici incorporati, oltre a tipi più complessi che rappresentano un'ampia gamma di costrutti logici, ad esempio il file system, le connessioni di rete, le raccolte e le matrici di oggetti e le date. Un tipico programma C# usa tipi dalla libreria di classi e tipi definiti dall'utente che modellano i concetti specifici del dominio relativo al problema del programma.  
  
Le informazioni archiviate in un tipo possono includere quanto segue:  
  
- Lo spazio di archiviazione richiesto da una variabile del tipo.  
  
- I valori minimi e massimi che può rappresentare.  
  
- I membri (metodi, campi, eventi e così via) in esso contenuti.  
  
- Il tipo di base da cui eredita.  
  
- Il percorso in cui viene allocata la memoria per le variabili in fase di esecuzione.  
  
- I tipi di operazioni consentite.  
  
Il compilatore usa le informazioni sul tipo per assicurarsi che tutte le operazioni eseguite nel codice siano *indipendenti dai tipi*. Se ad esempio si dichiara una variabile di tipo [int](language-reference/keywords/int.md), il compilatore consente di usare la variabile anche in operazioni di addizione e sottrazione. Se si prova a eseguire le stesse operazioni su una variabile di tipo [bool](language-reference/keywords/bool.md), il compilatore genera un errore, come illustrato nell'esempio seguente:  
  
[!code-csharp[Type Safety](../../samples/snippets/csharp/concepts/basic-types/type-safety.cs)]  
  
> [!NOTE]  
> Gli sviluppatori C e C++ devono tenere presente che, in C#, [bool](language-reference/keywords/bool.md) non è convertibile in [int](language-reference/keywords/int.md).  
  
Il compilatore incorpora le informazioni sul tipo nel file eseguibile come metadati. Common Language Runtime (CLR) usa i metadati in fase di esecuzione per garantire ulteriormente l'indipendenza dai tipi quando alloca e recupera la memoria.  

## <a name="specifying-types-in-variable-declarations"></a>Specifica dei tipi nelle dichiarazioni di variabile

Quando si dichiara una variabile o costante in un programma, è necessario specificarne il tipo oppure usare la parola chiave [var](language-reference/keywords/var.md) per consentire al compilatore di dedurre il tipo. L'esempio seguente illustra alcune dichiarazioni di variabili che usano sia tipi numerici incorporati sia tipi complessi definiti dall'utente:  
  
[!code-csharp[Variable Declaration](../../samples/snippets/csharp/concepts/basic-types/variable-declaration.cs)]  
  
I tipi di parametri e valori restituiti del metodo sono specificati nella firma del metodo. La firma seguente illustra un metodo che richiede un [int](language-reference/keywords/int.md) come argomento di input e restituisce una stringa:  
  
[!code-csharp[Method Signature](../../samples/snippets/csharp/concepts/basic-types/method-signature.cs)]  
  
Una variabile dichiarata non può essere dichiarata una seconda volta con un tipo nuovo e non è possibile assegnare a tale variabile un valore non compatibile con il relativo tipo dichiarato. Non è possibile, ad esempio, dichiarare una variabile [int](language-reference/keywords/int.md) e assegnare ad essa il valore booleano [true](language-reference/keywords/true-literal.md). I valori possono tuttavia essere convertiti in altri tipi, ad esempio quando vengono assegnati a nuove variabili o passati come argomenti di metodo. Una *conversione del tipo* che non causa la perdita di dati viene eseguita automaticamente dal compilatore, Una conversione che potrebbe causare la perdita di dati richiede un *cast* nel codice sorgente.

Per altre informazioni, vedere [Cast e conversioni di tipi](programming-guide/types/casting-and-type-conversions.md).

## <a name="built-in-types"></a>Tipi incorporati

Il linguaggio C# offre un set standard di tipi numerici predefiniti per rappresentare numeri interi, valori a virgola mobile, espressioni booleane, caratteri di testo, valori decimali e altri tipi di dati. Sono anche disponibili tipi **string** e **object** predefiniti. Questi possono essere usati in qualsiasi programma C#. Per altre informazioni sui tipi incorporati, vedere [Tabella dei tipi incorporati](language-reference/keywords/built-in-types-table.md).  
  
## <a name="custom-types"></a>Tipi personalizzati

Usare i costrutti [struct](language-reference/keywords/class.md), [class](language-reference/keywords/class.md), [interface](language-reference/keywords/interface.md) e [enum](language-reference/keywords/enum.md) per creare tipi personalizzati. La libreria di classi .NET Framework è una raccolta di tipi personalizzati offerti da Microsoft che è possibile usare nelle nuove applicazioni. Per impostazione predefinita, i tipi più comunemente usati nella libreria di classi sono disponibili in qualsiasi programma C#, mentre altri diventano disponibili solo quando si aggiunge in modo esplicito un riferimento di progetto all'assembly in cui sono definiti. Nel momento in cui il compilatore ha un riferimento all'assembly, è possibile dichiarare variabili (e costanti) dei tipi dichiarati nell'assembly in codice sorgente.
  
## <a name="generic-types"></a>Tipi generici

Un tipo può essere dichiarato con uno o più *parametri di tipo* che fungono da segnaposto per il tipo effettivo (*tipo concreto*) che il codice client specifica quando si crea un'istanza del tipo. Tali tipi sono definiti *tipi generici*. Ad esempio, il tipo <xref:System.Collections.Generic.List%601> di .NET Framework ha un solo parametro a cui, per convenzione, viene assegnato il nome *T*. Quando si crea un'istanza del tipo, si specifica il tipo degli oggetti che saranno contenuti nell'elenco, ad esempio, string:  
  
[!code-csharp[Generic types](../../samples/snippets/csharp/concepts/basic-types/generic-type.cs)]
  
L'uso del parametro di tipo consente di riutilizzare la stessa classe per contenere qualsiasi tipo di elemento senza dover convertire ogni elemento in [object](language-reference/keywords/object.md). Le classi di raccolte generiche sono definite *raccolte fortemente tipizzate* perché il compilatore conosce il tipo specifico degli elementi della raccolta e può generare un errore in fase di compilazione se, ad esempio, si prova ad aggiungere un numero intero all'oggetto `strings` nell'esempio precedente. Per altre informazioni, vedere [Generics](programming-guide/generics/index.md).

## <a name="implicit-types-anonymous-types-and-tuple-types"></a>Tipi impliciti, tipi anonimi e tipi di tupla

Come indicato in precedenza, è possibile digitare una variabile locale in modo implicito usando la parola chiave [var](language-reference/keywords/var.md). A tale variabile viene comunque assegnato un tipo in fase di compilazione, ma il tipo viene specificato dal compilatore. Per altre informazioni, vedere [Variabili locali tipizzate in modo implicito](programming-guide/classes-and-structs/implicitly-typed-local-variables.md).  
  
In alcuni casi non è consigliabile creare un tipo denominato per set semplici di valori correlati che non si intende archiviare o passare fuori dai limiti del metodo. A questo scopo è possibile creare *tipi anonimi*. Per altre informazioni, vedere [Tipi anonimi](programming-guide/classes-and-structs/anonymous-types.md).

È prassi comune voler restituire più valori da un metodo. È possibile creare *tipi di tupla* che restituiscono più valori in una singola chiamata al metodo. Per altre informazioni, vedere [Tuple](tuples.md)

## <a name="the-common-type-system"></a>Sistema dei tipi comuni

È importante capire due punti fondamentali sul sistema dei tipi in .NET Framework:  
  
- Il tipo supporta il principio di ereditarietà. I tipi possono derivare da altri tipi, denominati *tipi di base*. Il tipo derivato eredita (con alcune limitazioni) metodi, proprietà e altri membri del tipo di base, Il tipo di base può a sua volta derivare da un altro tipo, nel quale caso il tipo derivato eredita i membri di entrambi i tipi di base nella gerarchia di ereditarietà. Tutti i tipi, inclusi i tipi numerici predefiniti, ad esempio <xref:System.Int32> (parola chiave C#: `int`), derivano in definitiva da un unico tipo di base, ovvero <xref:System.Object> (parola chiave C#: `object`). Questa gerarchia di tipi unificati viene chiamata [Common Type System](../standard/common-type-system.md) (CTS). Per altre informazioni sull'ereditarietà in C#, vedere [Ereditarietà](programming-guide/classes-and-structs/inheritance.md).  
  
- Nel CTS ogni tipo è definito come *tipo valore* o *tipo riferimento*. Ciò include tutti i tipi personalizzati nella libreria di classi .NET Framework, nonché i tipi definiti dall'utente. I tipi definiti tramite la parola chiave [struct](language-reference/keywords/struct.md) sono tipi di valore e tutti i tipi numerici predefiniti sono tipi **struct**. Per altre informazioni sui tipi di valori, vedere [Struct](structs.md). I tipi definiti tramite la parola chiave [class](language-reference/keywords/class.md) sono tipi di riferimento. Per altre informazioni sui tipi di riferimento, vedere [Classi](classes.md). I tipi di riferimento e i tipi di valore hanno regole diverse e un comportamento diverso in fase di esecuzione.

## <a name="see-also"></a>Vedere anche

- [Struct](structs.md)
- [Classi](classes.md)
