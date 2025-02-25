---
title: Clausola where (Riferimento C#)
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- whereclause_CSharpKeyword
helpviewer_keywords:
- where keyword [C#]
- where clause [C#]
ms.assetid: 7f9bf952-7744-4f91-b676-cddb55d107c3
ms.openlocfilehash: fc259f0e0a83d2f55bf2d50fa336c9201b8b5bef
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2019
ms.locfileid: "65633209"
---
# <a name="where-clause-c-reference"></a>Clausola where (Riferimento C#)

La clausola `where` viene usata in un'espressione di query per specificare quali elementi dell'origine dati verranno restituiti nell'espressione di query. Viene applicata una condizione booleana (*predicato*) a ogni elemento di origine (a cui fa riferimento la variabile di intervallo) e viene restituita quella per cui la condizione specificata è vera. Una singola espressione di query può contenere più clausole `where` e una singola clausola può contenere più sottoespressioni di predicato.

## <a name="example"></a>Esempio

Nell'esempio seguente la clausola `where` esclude tutti i numeri tranne quelli minori di cinque. Se si rimuove la clausola `where`, vengono restituiti tutti i numeri dell'origine dati. L'espressione `num < 5` è il predicato che viene applicato a ogni elemento.

[!code-csharp[cscsrefQueryKeywords#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Where.cs#5)]

## <a name="example"></a>Esempio

In una singola clausola `where` è possibile specificare tutti i predicati necessari usando gli operatori [ && ](../operators/boolean-logical-operators.md#conditional-logical-and-operator-) e [&#124;&#124;](../operators/boolean-logical-operators.md#conditional-logical-or-operator-). Nell'esempio seguente la query specifica due predicati per selezionare solo i numeri pari minori di cinque.

[!code-csharp[cscsrefQueryKeywords#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Where.cs#6)]  

## <a name="example"></a>Esempio

Una clausola `where` può contenere uno o più metodi che restituiscono valori booleani. Nell'esempio seguente la clausola `where` usa un metodo per determinare se il valore corrente della variabile di intervallo è pari o dispari.

[!code-csharp[cscsrefQueryKeywords#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Where.cs#7)]

## <a name="remarks"></a>Osservazioni

La clausola `where` è un meccanismo di filtro. Può essere posizionata praticamente ovunque in un'espressione di query, ma non può essere la prima o l'ultima clausola. Una clausola `where` la può apparire prima o dopo una clausola [group](group-clause.md) a seconda se gli elementi di origine devono essere filtrati prima o dopo essere stati raggruppati.

Se un predicato specificato non è valido per gli elementi nell'origine dati, si verificherà un errore in fase di compilazione. Questo è uno dei vantaggi del controllo efficace del tipo offerto da [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)].

In fase di compilazione, la parola chiave `where` viene convertita in una chiamata al metodo <xref:System.Linq.Enumerable.Where%2A> dell'operatore query standard.

## <a name="see-also"></a>Vedere anche

- [Parole chiave di query (LINQ)](query-keywords.md)
- [Clausola from](from-clause.md)
- [Clausola select](select-clause.md)
- [Filtraggio dei dati](../../programming-guide/concepts/linq/filtering-data.md)
- [Espressioni di query LINQ](../../../csharp/programming-guide/linq-query-expressions/index.md)
- [Nozioni di base su LINQ in C#](../../programming-guide/concepts/linq/getting-started-with-linq.md)
