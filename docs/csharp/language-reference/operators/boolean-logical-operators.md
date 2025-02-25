---
title: Operatori logici booleani - Riferimenti per C#
description: Informazioni sugli operatori C# che eseguono operazioni logiche di negazione, congiunzione (AND) e disgiunzione inclusiva ed esclusiva (OR) con operandi booleani.
ms.date: 04/08/2019
author: pkulikov
f1_keywords:
- '!_CSharpKeyword'
- '&_CSharpKeyword'
- ^_CSharpKeyword
- '|_CSharpKeyword'
- '&&_CSharpKeyword'
- '||_CSharpKeyword'
helpviewer_keywords:
- logical operators [C#]
- short-circuiting operators [C#]
- logical negation operator [C#]
- NOT operator [C#]
- '! operator [C#]'
- AND operator [C#]
- ampersand operator [C#]
- conjunction operator [C#]
- '& operator [C#]'
- exclusive OR operator [C#]
- XOR operator [C#]
- ^ operator [C#]
- OR operator [C#]
- disjunction operator [C#]
- '| operator [C#]'
- conditional AND operator [C#]
- short-circuiting AND operator [C#]
- '&& operator [C#]'
- conditional OR operator [C#]
- short-circuiting OR operator [C#]
- '|| operator [C#]'
ms.openlocfilehash: d94552d9a1acfdd63b9694810e724c4347e615e7
ms.sourcegitcommit: 904b98d8d706f0e2d5ceaa00ce17ffbd92adfb88
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66758286"
---
# <a name="boolean-logical-operators-c-reference"></a>Operatori logici booleani (riferimenti per C#)

Gli operatori seguenti eseguono operazioni logiche con gli operandi [bool](../keywords/bool.md):

- Operatore unario [`!` (negazione logica)](#logical-negation-operator-).
- Operatori binari [`&` (AND logico)](#logical-and-operator-), [`|` (OR logico)](#logical-or-operator-) e [`^` (OR esclusivo logico)](#logical-exclusive-or-operator-). Questi operatori valutano sempre entrambi gli operandi.
- Operatori binari [`&&` (AND condizionale logico)](#conditional-logical-and-operator-) e [`||` (OR condizionale logico)](#conditional-logical-or-operator-). Questi operatori valutano il secondo operando solo se è necessario.

Per gli operandi dei tipi [integrali](../keywords/integral-types-table.md), gli operatori `&`, `|` e `^` eseguono operazioni logiche bit per bit. Per altre informazioni, vedere [Operatori di scorrimento e bit per bit](bitwise-and-shift-operators.md).

## <a name="logical-negation-operator-"></a>Operatore di negazione logica !

L'operatore `!` calcola la negazione logica del suo operando. Vale a dire, produce `true` se l'operando restituisce `false` e `false` se l'operando restituisce `true`:

[!code-csharp-interactive[logical negation](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Negation)]

## <a name="logical-and-operator-amp"></a>Operatore AND logico &amp;

L'operatore `&` calcola l'AND logico dei relativi operandi. Il risultato di `x & y` è `true` se `x` e `y` restituiscono `true`. In caso contrario, il risultato è `false`.

L'operatore `&` valuta entrambi gli operandi anche se il primo operando restituisce `false`, in modo che il risultato sia `false` indipendentemente dal valore del secondo operando.

Nell'esempio seguente il secondo operando dell'operatore `&` è una chiamata a un metodo, che viene eseguita indipendentemente dal valore del primo operando:

[!code-csharp-interactive[logical AND](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#And)]

L'[operatore AND condizionale logico](#conditional-logical-and-operator-) `&&` calcola anche l'AND logico dei relativi operandi, ma non valuta il secondo operando se il primo operando restituisce `false`.

Per gli operandi dei tipi integrali, l'operatore `&` calcola l'[AND logico bit per bit](bitwise-and-shift-operators.md#logical-and-operator-) dei relativi operandi. L'operatore unario `&` è l'[operatore address-of](pointer-related-operators.md#address-of-operator-).

## <a name="logical-exclusive-or-operator-"></a>Operatore OR esclusivo logico: ^

L'operatore `^` calcola l'OR esclusivo logico, noto anche come XOR logico, dei relativi operandi. Il risultato di `x ^ y` è `true` se `x` restituisce `true` e `y` restituisce `false` oppure `x` restituisce `false` e `y` restituisce `true`. In caso contrario, il risultato è `false`. Vale a dire, per gli operandi `bool`, l'operatore `^` calcola lo stesso risultato dell'[operatore di disuguaglianza](equality-operators.md#inequality-operator-) `!=`.

[!code-csharp-interactive[logical exclusive OR](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Xor)]

Per gli operandi dei tipi integrali, l'operatore `^` calcola l'[OR esclusivo logico bit per bit](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) dei relativi operandi.

## <a name="logical-or-operator-"></a>Operatore OR logico |

L'operatore `|` calcola l'OR logico dei relativi operandi. Il risultato di `x | y` è `true` se `x` o `y` restituisce `true`. In caso contrario, il risultato è `false`.

L'operatore `|` valuta entrambi gli operandi anche se il primo operando restituisce `true`, in modo che il risultato sia `true` indipendentemente dal valore del secondo operando.

Nell'esempio seguente il secondo operando dell'operatore `|` è una chiamata a un metodo, che viene eseguita indipendentemente dal valore del primo operando:

[!code-csharp-interactive[logical OR](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Or)]

L'[operatore OR condizionale logico](#conditional-logical-or-operator-) `||` calcola anche l'OR logico dei relativi operandi, ma non valuta il secondo operando se il primo operando restituisce `true`.

Per gli operandi dei tipi integrali, l'operatore `|` calcola l'[OR logico bit per bit](bitwise-and-shift-operators.md#logical-or-operator-) dei relativi operandi.

## <a name="conditional-logical-and-operator-ampamp"></a>Operatore AND condizionale logico&amp;&amp;

L'operatore AND condizionale logico `&&`, chiamato anche operatore AND logico di "corto circuito", calcola l'AND logico dei relativi operandi. Il risultato di `x && y` è `true` se `x` e `y` restituiscono `true`. In caso contrario, il risultato è `false`. Se il primo operando restituisce `false`, il secondo operando non viene valutato.

Nell'esempio seguente il secondo operando dell'operatore `&&` è una chiamata a un metodo, che non viene eseguita se il primo operando restituisce `false`:

[!code-csharp-interactive[conditional logical AND](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#ConditionalAnd)]

L'[operatore AND logico](#logical-and-operator-) `&` calcola anche l'AND logico dei relativi operandi, ma valuta sempre entrambi gli operandi.

## <a name="conditional-logical-or-operator-"></a>Operatore OR condizionale logico ||

L'operatore OR condizionale logico `||`, chiamato anche operatore OR logico di "corto circuito", calcola l'OR logico dei relativi operandi. Il risultato di `x || y` è `true` se `x` o `y` restituisce `true`. In caso contrario, il risultato è `false`. Se il primo operando restituisce `true`, il secondo operando non viene valutato.

Nell'esempio seguente il secondo operando dell'operatore `||` è una chiamata a un metodo, che non viene eseguita se il primo operando restituisce `true`:

[!code-csharp-interactive[conditional logical OR](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#ConditionalOr)]

L'[operatore OR logico](#logical-or-operator-) `|` calcola anche l'OR logico dei relativi operandi, ma valuta sempre entrambi gli operandi.

## <a name="nullable-boolean-logical-operators"></a>Operatori logici booleani nullable

Per gli operandi `bool?`, gli operatori `&` e `|` supportano la logica a tre valori. La semantica di questi operatori è definita dalla tabella seguente:  
  
|x|y|x&y|x&#124;y|  
|----|----|----|----|  
|true|true|true|true|  
|true|False|false|true|  
|true|Null|Null|true|  
|False|true|False|true|  
|False|False|False|False|  
|False|Null|False|Null|  
|Null|true|Null|true|  
|Null|False|False|Null|  
|Null|Null|Null|Null|  

Il comportamento di questi operatori è diverso dal comportamento tipico degli operatori con tipi valore nullable. In genere un operatore definito per gli operandi di un tipo valore può essere usato anche con gli operandi del tipo valore nullable corrispondente. Un operatore di questo tipo produce `null` se uno qualsiasi dei suoi operandi è `null`. Tuttavia gli operatori `&` e `|` possono produrre valori non Null anche se uno degli operandi è `null`. Per altre informazioni sul comportamento degli operatori con i tipi valore nullable, vedere la sezione [Operatori](../../programming-guide/nullable-types/using-nullable-types.md#operators) dell'articolo [Uso dei tipi nullable](../../programming-guide/nullable-types/using-nullable-types.md).

È anche possibile usare gli operatori `!` e `^` con gli operandi `bool?`, come mostrato nell'esempio seguente:

[!code-csharp-interactive[lifted negation and xor](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#WithNullableBoolean)]

Gli operatori condizionali logici `&&` e `||` non supportano gli operandi `bool?`.

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

Gli operatori `&`, `|` e `^` supportano l'assegnazione composta, come illustrato nell'esempio seguente:

[!code-csharp-interactive[compound assignment](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#CompoundAssignment)]

Gli operatori condizionali logici `&&` e `||` non supportano l'assegnazione composta.

## <a name="operator-precedence"></a>Precedenza tra gli operatori

Nell'elenco seguente gli operatori logici sono ordinati dalla precedenza più elevata a quella più bassa:

- Operatore di negazione logica `!`
- Operatore AND logico `&`
- Operatore OR esclusivo logico `^`
- Operatore OR logico `|`
- Operatore AND condizionale logico `&&`
- Operatore OR condizionale logico `||`

Usare le parentesi, `()`, per cambiare l'ordine di valutazione imposto dalla precedenza tra gli operatori:

[!code-csharp-interactive[operator precedence](~/samples/csharp/language-reference/operators/BooleanLogicalOperators.cs#Precedence)]

Per l'elenco completo degli operatori C# ordinati in base al livello di precedenza, vedere [Operatori C#](index.md).

## <a name="operator-overloadability"></a>Overload degli operatori

Un tipo definito dall'utente può eseguire l'[overload](../keywords/operator.md) degli operatori `!`, `&`, `|` e `^`. Quando viene eseguito l'overload di un operatore binario, viene anche eseguito in modo implicito l'overload dell'operatore di assegnazione composta corrispondente. Un tipo definito dall'utente non può eseguire in modo esplicito l'overload di un operatore di assegnazione composta.

Un tipo definito dall'utente non può eseguire l'overload degli operatori condizionali logici `&&` e `||`. Tuttavia, se un tipo definito dall'utente esegue l'overload degli [operatori true e false](true-false-operators.md) e dell'operatore `&` o `|` in un determinato modo, l'operazione `&&` o `||` può essere valutata per gli operandi di quel tipo. Per altre informazioni, vedere la sezione [Operatori logici condizionali definiti dall'utente](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators) di [Specifica del linguaggio C#](~/_csharplang/spec/introduction.md).

## <a name="c-language-specification"></a>Specifiche del linguaggio C#

Per altre informazioni, vedere le sezioni seguenti delle [specifiche del linguaggio C#](~/_csharplang/spec/introduction.md):

- [Operatore di negazione logica](~/_csharplang/spec/expressions.md#logical-negation-operator)
- [Operatori logici](~/_csharplang/spec/expressions.md#logical-operators)
- [Operatori logici condizionali](~/_csharplang/spec/expressions.md#conditional-logical-operators)
- [Assegnazione composta](~/_csharplang/spec/expressions.md#compound-assignment)

## <a name="see-also"></a>Vedere anche

- [Riferimenti per C#](../index.md)
- [Guida per programmatori C#](../../programming-guide/index.md)
- [Operatori C#](index.md)
- [Operatori di scorrimento e bit per bit](bitwise-and-shift-operators.md)
