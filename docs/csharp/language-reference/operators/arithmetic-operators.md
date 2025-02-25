---
title: Operatori aritmetici - Riferimenti per C#
description: Informazioni sugli operatori C# che eseguono operazioni di moltiplicazione, divisione, resto, addizione e sottrazione con i tipi numerici.
ms.date: 03/27/2019
author: pkulikov
f1_keywords:
- ++_CSharpKeyword
- --_CSharpKeyword
- '*_CSharpKeyword'
- /_CSharpKeyword
- '%_CSharpKeyword'
- +_CSharpKeyword
- -_CSharpKeyword
helpviewer_keywords:
- arithmetic operators [C#]
- increment operator [C#]
- ++ operator [C#]
- decrement operator [C#]
- -- operator [C#]
- multiplication operator [C#]
- '* operator [C#]'
- division operator [C#]
- / operator [C#]
- remainder operator [C#]
- '% operator [C#]'
- addition operator [C#]
- + operator [C#]
- subtraction operator [C#]
- '- operator [C#]'
ms.openlocfilehash: 69b4f3fb52ce5da25081f3da7a8909151e1492f6
ms.sourcegitcommit: 904b98d8d706f0e2d5ceaa00ce17ffbd92adfb88
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66758415"
---
# <a name="arithmetic-operators-c-reference"></a>Operatori aritmetici (Riferimenti per C#)

Gli operatori seguenti eseguono operazioni aritmetiche con i tipi numerici:

- Operatori unari [`++` (incremento)](#increment-operator-), [`--` (decremento)](#decrement-operator---), [`+` (più)](#unary-plus-and-minus-operators) e [`-` (meno)](#unary-plus-and-minus-operators)
- Operatori binari [`*` (moltiplicazione)](#multiplication-operator-), [`/` (divisione)](#division-operator-), [`%` (resto)](#remainder-operator-), [`+` (addizione)](#addition-operator-) e [`-` (sottrazione)](#subtraction-operator--)

Questi operatori supportano tutti i tipi numerici [integrali](../keywords/integral-types-table.md) e [a virgola mobile](../keywords/floating-point-types-table.md).

## <a name="increment-operator-"></a>Operatore di incremento ++

L'operatore di incremento unario `++` incrementa il suo operando di 1. L'operando deve essere una variabile, un accesso a una [proprietà](../../programming-guide/classes-and-structs/properties.md) o un accesso a un [indicizzatore](../../../csharp/programming-guide/indexers/index.md).

L'operatore di incremento è supportato in due forme: l'operatore di incremento suffisso, `x++`, e l'operatore di incremento prefisso, `++x`.

### <a name="postfix-increment-operator"></a>Operatore di incremento suffisso

Il risultato di `x++` è il valore di `x` *prima* dell'operazione, come illustrato nell'esempio seguente:

[!code-csharp-interactive[postfix increment](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#PostfixIncrement)]

### <a name="prefix-increment-operator"></a>Operatore di incremento prefisso

Il risultato di `++x` è il valore di `x` *dopo* l'operazione, come illustrato nell'esempio seguente:

[!code-csharp-interactive[prefix increment](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#PrefixIncrement)]

## <a name="decrement-operator---"></a>Operatore di decremento --

L'operatore di decremento unario `--` decrementa il proprio operando di 1. L'operando deve essere una variabile, un accesso a una [proprietà](../../programming-guide/classes-and-structs/properties.md) o un accesso a un [indicizzatore](../../../csharp/programming-guide/indexers/index.md).

L'operatore di decremento è supportato in due forme: l'operatore di decremento suffisso, `x--`, e l'operatore di decremento prefisso, `--x`.

### <a name="postfix-decrement-operator"></a>Operatore di decremento suffisso

Il risultato di `x--` è il valore di `x` *prima* dell'operazione, come illustrato nell'esempio seguente:

[!code-csharp-interactive[postfix decrement](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#PostfixDecrement)]

### <a name="prefix-decrement-operator"></a>Operatore di decremento prefisso

Il risultato di `--x` è il valore di `x` *dopo* l'operazione, come illustrato nell'esempio seguente:

[!code-csharp-interactive[prefix decrement](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#PrefixDecrement)]

## <a name="unary-plus-and-minus-operators"></a>Operatori più e meno unari

L'operatore `+` unario restituisce il valore del proprio operando. L'operatore `-` unario calcola la negazione numerica del proprio operando.

[!code-csharp-interactive[unary plus and minus](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#UnaryPlusAndMinus)]

L'operatore `-` unario non supporta il tipo [ulong](../keywords/ulong.md).

## <a name="multiplication-operator-"></a>Operatore di moltiplicazione *

L'operatore di moltiplicazione `*` calcola il prodotto degli operandi:

[!code-csharp-interactive[multiplication operator](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#Multiplication)]

L'operatore `*` unario è l'[operatore di riferimento indiretto a puntatore](pointer-related-operators.md#pointer-indirection-operator-).

## <a name="division-operator-"></a>Operatore di divisione /

L'operatore di divisione `/` divide il primo operando per il secondo operando.

### <a name="integer-division"></a>Divisione di interi

Per gli operandi di tipo Integer, il risultato dell'operatore `/` è di tipo Integer ed è uguale al quoziente dei due operandi arrotondato allo zero:

[!code-csharp-interactive[integer division](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#IntegerDivision)]

Per ottenere il quoziente dei due operandi come numero a virgola mobile, usare il tipo `float`, `double`,o `decimal`:

[!code-csharp-interactive[integer as floating-point division](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#IntegerAsFloatingPointDivision)]

### <a name="floating-point-division"></a>Divisione a virgola mobile

Per i tipi `float`, `double` e `decimal`, il risultato dell'operatore `/` è il quoziente dei due operandi:

[!code-csharp-interactive[floating-point division](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#FloatingPointDivision)]

Se uno degli operandi è `decimal`, un altro operando non può essere né `float` né `double`, perché né `float` né `double` è convertibile implicitamente in `decimal`. È necessario convertire esplicitamente l'operando `float` o `double` nel tipo `decimal`. Per altre informazioni sulle conversioni implicite tra tipi numerici, vedere [Tabella delle conversioni numeriche implicite](../keywords/implicit-numeric-conversions-table.md).

## <a name="remainder-operator-"></a>Operatore di resto %

L'operatore di resto `%` calcola il resto dopo aver diviso il primo operando per il secondo.

### <a name="integer-remainder"></a>Resto intero
  
Per gli operandi di tipi interi, il risultato di `a % b` è il valore prodotto da `a - (a / b) * b`. Il segno del resto diverso da zero è uguale a quello del primo operando, come illustrato nell'esempio seguente:

[!code-csharp-interactive[integer remainder](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#IntegerRemainder)]

Usare il metodo <xref:System.Math.DivRem%2A?displayProperty=nameWithType> per calcolare sia la divisione di numeri interi, sia i risultati del resto.

### <a name="floating-point-remainder"></a>Resto a virgola mobile

Per gli operandi `float` e `double`, il risultato di `x % y` per `x` e `y` finiti è il valore `z` tale che

- Il segno di `z`, se diverso da zero, è uguale a quello di `x`.
- Il valore assoluto di `z` è il valore prodotto da `|x| - n * |y|` in cui `n` è l'intero più grande possibile, che è minore o uguale a `|x| / |y|` e `|x|` e `|y|` sono i valori assoluti di `x` e `y`, rispettivamente.

> [!NOTE]
> Questo metodo di calcolo del resto è analogo a quello utilizzato per gli operandi di numero intero, ma differisce da IEEE 754. Se è necessario che l'operazione di resto sia compatibile con il valore IEEE 754, usare il metodo <xref:System.Math.IEEERemainder%2A?displayProperty=nameWithType>.

Per informazioni sul comportamento dell'operatore `%` in caso di operandi non finiti, vedere la sezione [Operatore di resto](~/_csharplang/spec/expressions.md#remainder-operator) della [specifica del linguaggio C#](~/_csharplang/spec/introduction.md).

Per gli operandi `decimal`, l'operatore di resto `%` equivale all'[operatore di resto](<xref:System.Decimal.op_Modulus(System.Decimal,System.Decimal)>) di tipo <xref:System.Decimal?displayProperty=nameWithType>.

L'esempio seguente illustra il comportamento dell'operatore di resto con operandi a virgola mobile:

[!code-csharp-interactive[floating-point remainder](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#FloatingPointRemainder)]

## <a name="addition-operator-"></a>Operatore di addizione +

L'operatore di addizione `+` calcola la somma degli operandi:

[!code-csharp-interactive[addition operator](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#Addition)]

È possibile usare l'operatore `+` anche per la concatenazione di stringhe e la combinazione di delegati. Per altre informazioni, vedere l'articolo sugli [operatori `+` e `+=`](addition-operator.md).

## <a name="subtraction-operator--"></a>Operatore di sottrazione -

L'operatore di sottrazione `-` sottrae il secondo operando dal primo:

[!code-csharp-interactive[subtraction operator](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#Subtraction)]

È possibile usare l'operatore `-` anche per la rimozione di delegati. Per altre informazioni, vedere l'articolo [Operatore `-`](subtraction-operator.md).

## <a name="compound-assignment"></a>Assegnazione composta

Per un operatore binario `op`, un'espressione di assegnazione composta in formato

```csharp
x op= y
```

equivale a

```csharp
x = x op y
```

con la differenza che `x` viene valutato una sola volta.

L'esempio seguente illustra l'uso dell'assegnazione composta con gli operatori aritmetici:

[!code-csharp-interactive[compound assignment](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#CompoundAssignment)]

In ragione delle [promozioni numeriche](~/_csharplang/spec/expressions.md#numeric-promotions) il risultato dell'operazione `op` potrebbe non essere convertibile in modo implicito nel tipo `T` di `x`. In questo caso, se `op` è un operatore già definito e il risultato dell'operazione è convertibile in modo esplicito nel tipo `T` di `x`, un'espressione di assegnazione composta nel formato `x op= y` equivale a `x = (T)(x op y)`, con la differenza che `x` viene valutato una sola volta. L'esempio seguente illustra questo comportamento:

[!code-csharp-interactive[compound assignment with cast](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#CompoundAssignmentWithCast)]

È possibile usare gli operatori `+=` e `-=` anche per la sottoscrizione e l'annullamento della sottoscrizione di [eventi](../keywords/event.md). Per altre informazioni, vedere [Procedura: Sottoscrivere e annullare la sottoscrizione di eventi](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md).

## <a name="operator-precedence-and-associativity"></a>Precedenza e associatività degli operatori

Nell'elenco seguente gli operatori aritmetici sono ordinati dalla precedenza più elevata a quella più bassa:

- Operatori di incremento `x++` e decremento `x--` in forma suffissa
- Operatori di incremento `++x` e decremento `--x` e unari `+` e `-`
- Operatori moltiplicativi `*`, `/` e `%`
- Operatori additivi `+` e `-`

Gli operatori aritmetici binari prevedono l'associazione all'operando sinistro. Ciò significa che gli operatori con lo stesso livello di precedenza vengono valutati da sinistra a destra.

Usare le parentesi, `()`, per cambiare l'ordine di valutazione imposto dalla precedenza e dall'associatività degli operatori.

[!code-csharp-interactive[precedence and associativity](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#PrecedenceAndAssociativity)]

Per l'elenco completo degli operatori C# ordinati in base al livello di precedenza, vedere [Operatori C#](index.md).

## <a name="arithmetic-overflow-and-division-by-zero"></a>Overflow aritmetico e divisione per zero

Quando il risultato di un'operazione aritmetica non rientra nell'intervallo di valori finiti possibili del tipo numerico interessato, il comportamento di un operatore aritmetico dipende dal tipo dei relativi operandi.

### <a name="integer-arithmetic-overflow"></a>Overflow aritmetico di numeri interi

La divisione di interi per zero genera sempre un'eccezione <xref:System.DivideByZeroException>.

In caso di overflow aritmetico di interi, un contesto di verifica overflow, che può essere [verificato o non verificato](../keywords/checked-and-unchecked.md), controlla il comportamento risultante:

- In un contesto verificato, se l'overflow si verifica in un'espressione costante, verrà restituito un errore in fase di compilazione. Quando invece l'operazione avviene in fase di esecuzione, viene generata una <xref:System.OverflowException>.
- In un contesto non verificato, il risultato viene troncato tramite la rimozione dei bit più significativi che non rientrano nel tipo di destinazione.

Oltre alle istruzioni [checked e unchecked](../keywords/checked-and-unchecked.md), è possibile usare gli operatori `checked` e `unchecked` per controllare il contesto di verifica overflow in cui viene valutata un'espressione:

[!code-csharp-interactive[checked and unchecked](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#CheckedUnchecked)]

Per impostazione predefinita, le operazioni aritmetiche vengono eseguite in un contesto *non verificato*.

### <a name="floating-point-arithmetic-overflow"></a>Overflow aritmetico di numeri a virgola mobile

Le operazioni aritmetiche con i tipi `float` e `double` non generano mai un'eccezione. Il risultato delle operazioni aritmetiche con questi tipi può essere uno dei valori speciali che rappresentano un numero infinito e non un numero:

[!code-csharp-interactive[double non-finite values](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#FloatingPointOverflow)]

Per gli operandi di tipo `decimal`, l'overflow aritmetico genera sempre una <xref:System.OverflowException> e la divisione per zero genera sempre una <xref:System.DivideByZeroException>.

## <a name="round-off-errors"></a>Errori di arrotondamento

A causa delle limitazioni generali della rappresentazione a virgola mobile di numeri reali e dell'aritmetica a virgola mobile, nei calcoli con tipi a virgola mobile possono verificarsi errori di arrotondamento. Ossia, il risultato prodotto di un'espressione può essere diverso dal risultato matematico previsto. L'esempio seguente illustra molti di questi casi:

[!code-csharp-interactive[round-off errors](~/samples/csharp/language-reference/operators/ArithmeticOperators.cs#RoundOffErrors)]

Per altre informazioni, vedere le osservazioni nelle pagine di riferimento [System.Double](/dotnet/api/system.double#remarks), [System.Single](/dotnet/api/system.single#remarks) o [System.Decimal](/dotnet/api/system.decimal#remarks).

## <a name="operator-overloadability"></a>Overload degli operatori

Un tipo definito dall'utente può eseguire l'[overload](../keywords/operator.md) degli operatori aritmetici unari (`++`, `--`, `+` e `-`) e binari (`*`, `/`, `%`, `+` e `-`). Quando viene eseguito l'overload di un operatore binario, viene anche eseguito in modo implicito l'overload dell'operatore di assegnazione composta corrispondente. Un tipo definito dall'utente non può eseguire in modo esplicito l'overload di un operatore di assegnazione composta.

## <a name="c-language-specification"></a>Specifiche del linguaggio C#

Per altre informazioni, vedere le sezioni seguenti delle [specifiche del linguaggio C#](~/_csharplang/spec/introduction.md):

- [Operatori di incremento e decremento in forma suffissa](~/_csharplang/spec/expressions.md#postfix-increment-and-decrement-operators)
- [Operatori di incremento e decremento in forma prefissa](~/_csharplang/spec/expressions.md#prefix-increment-and-decrement-operators)
- [Operatore più unario](~/_csharplang/spec/expressions.md#unary-plus-operator)
- [Operatore meno unario](~/_csharplang/spec/expressions.md#unary-minus-operator)
- [Operatore di moltiplicazione](~/_csharplang/spec/expressions.md#multiplication-operator)
- [Operatore di divisione](~/_csharplang/spec/expressions.md#division-operator)
- [Operatore di resto](~/_csharplang/spec/expressions.md#remainder-operator)
- [Operatore addizione](~/_csharplang/spec/expressions.md#addition-operator)
- [Operatore di sottrazione](~/_csharplang/spec/expressions.md#subtraction-operator)
- [Assegnazione composta](~/_csharplang/spec/expressions.md#compound-assignment)
- [Operatori Checked e Unchecked](~/_csharplang/spec/expressions.md#the-checked-and-unchecked-operators)
- [Promozioni numeriche](~/_csharplang/spec/expressions.md#numeric-promotions)

## <a name="see-also"></a>Vedere anche

- [Riferimenti per C#](../index.md)
- [Guida per programmatori C#](../../programming-guide/index.md)
- [Operatori C#](index.md)
- <xref:System.Math?displayProperty=nameWithType>
- <xref:System.MathF?displayProperty=nameWithType>
- [Dati numerici in .NET](../../../standard/numerics.md)
