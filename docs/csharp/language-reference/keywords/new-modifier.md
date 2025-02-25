---
title: Modificatore new - Riferimenti per C#
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- new modifier keyword [C#]
ms.assetid: a2e20856-33b9-4620-b535-a60dbce8349b
ms.openlocfilehash: 3a642996da8f0126e59e21d3553a7d8ba73dab23
ms.sourcegitcommit: 10986410e59ff29f2ec55c6759bde3eb4d1a00cb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66422675"
---
# <a name="new-modifier-c-reference"></a>Modificatore new (Riferimenti per C#)

Se usata come modificatore di dichiarazione, la parola chiave `new` nasconde in modo esplicito un membro ereditato da una classe base. Quando si nasconde un membro ereditato, la versione derivata del membro sostituisce la versione della classe base. Sebbene sia possibile nascondere i membri senza usare il modificatore `new`, viene visualizzato un avviso del compilatore. Se si usa `new` in modo esplicito per nascondere un membro, esso elimina l'avviso.

Per nascondere un membro ereditato, dichiararlo nella classe derivata usando lo stesso nome di membro e modificarlo con la parola chiave `new`. Ad esempio:

[!code-csharp[csrefKeywordsOperator#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#8)]

In questo esempio `BaseC.Invoke` è nascosto da `DerivedC.Invoke`. Il campo `x` non è interessato perché non è nascosto da un nome simile.

Un nome nascosto tramite ereditarietà accetta uno dei formati seguenti:

- In genere, una costante, un campo, una proprietà o un tipo introdotto in una classe o uno struct nasconde tutti i membri della classe base che condividono il nome.  Esistono casi particolari.  Se, ad esempio, si dichiara che un nuovo campo con il nome `N` dispone di un tipo non richiamabile e un tipo di base dichiara che `N` sia un metodo, il nuovo campo non nasconde la dichiarazione di base nella sintassi di chiamata.  Per informazioni dettagliate, vedere la sezione "Member Lookup" (Ricerca di membri) nella sezione "Expressions" (Espressioni) in [C# 5.0 language specification](https://www.microsoft.com/download/details.aspx?id=7029) (Specifiche del linguaggio C# 5.0).

- Un metodo inserito in una classe o uno struct nasconde proprietà, campi e tipi che condividono il nome con la classe base. Nasconde inoltre tutti i metodi della classe base con la stessa firma.

- Un indicizzatore inserito in una classe o uno struct nasconde tutti gli indicizzatori della classe base con la stessa firma.

Non è possibile usare sia `new` sia [override](override.md) nello stesso membro, in quanto i significati dei due modificatori si escludono reciprocamente. Il modificatore `new` crea un nuovo membro con lo stesso nome e fa sì che il membro originale venga nascosto. Il modificatore `override` estende l'implementazione per un membro ereditato.

L'utilizzo del modificatore `new` in una dichiarazione che non nasconde un membro ereditato genera un avviso.

## <a name="example"></a>Esempio

In questo esempio, una classe base, `BaseC`, e una classe derivata, `DerivedC`, usano lo stesso nome di campo `x`, che nasconde il valore del campo ereditato. Nell'esempio viene illustrato l'utilizzo del modificatore `new`. Viene inoltre descritto come accedere ai membri nascosti della classe base usando i relativi nomi completi.

[!code-csharp[csrefKeywordsOperator#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#9)]

## <a name="example"></a>Esempio

In questo esempio, una classe annidata nasconde una classe che ha lo stesso nome nella classe base. L'esempio illustra come usare il modificatore `new` per eliminare il messaggio di avviso e come accedere ai membri di classe nascosti tramite i relativi nomi completi.

[!code-csharp[csrefKeywordsOperator#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#10)]

Se si rimuove il modificatore `new`, la compilazione e l'esecuzione del programma saranno comunque possibili, ma verrà visualizzato il messaggio di avviso riportato di seguito:

```
The keyword new is required on 'MyDerivedC.x' because it hides inherited member 'MyBaseC.x'.
```

## <a name="c-language-specification"></a>Specifiche del linguaggio C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Vedere anche

- [Riferimenti per C#](../../language-reference/index.md)
- [Guida per programmatori C#](../../programming-guide/index.md)
- [Parole chiave di C#](index.md)
- [Modificatori](modifiers.md)
- [Controllo delle versioni con le parole chiave Override e New](../../programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords.md)
- [Sapere quando usare le parole chiave Override e New](../../programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md)
