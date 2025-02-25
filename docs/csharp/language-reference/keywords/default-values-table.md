---
title: Tabella dei valori predefiniti - Riferimenti per C#
ms.custom: seodec18
description: Informazioni sui valori predefiniti dei tipi valore C#.
ms.date: 08/23/2018
helpviewer_keywords:
- constructors [C#], return values
- keywords [C#], new
- parameterless constructor [C#]
- defaults [C#]
- value types [C#], initializing
- variables [C#], value types
- constructors [C#], parameterless constructor
- types [C#], parameterless constructor return values
ms.openlocfilehash: 4fc9a35f69540e047a97c21788015ca8e54068a0
ms.sourcegitcommit: 10986410e59ff29f2ec55c6759bde3eb4d1a00cb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66422027"
---
# <a name="default-values-table-c-reference"></a>Tabella dei valori predefiniti (Riferimenti per C#)

La tabella seguente mostra i valori predefiniti dei [tipi valore](value-types.md).

|Tipo valore|Valore predefinito|
|----------------|-------------------|
|[bool](bool.md)|`false`|
|[byte](byte.md)|0|
|[char](char.md)|'\0'|
|[decimal](decimal.md)|0M|
|[double](double.md)|0.0D|
|[enum](enum.md)|Valore prodotto dall'espressione `(E)0`, dove `E` è l'identificatore di enumerazione.|
|[float](float.md)|0.0F|
|[int](int.md)|0|
|[long](long.md)|0L|
|[sbyte](sbyte.md)|0|
|[short](short.md)|0|
|[struct](struct.md)|Valore prodotto impostando tutti i campi dei tipi valore sui rispettivi valori predefiniti e tutti i campi dei tipi riferimento su `null`.|
|[uint](uint.md)|0|
|[ulong](ulong.md)|0|
|[ushort](ushort.md)|0|

## <a name="remarks"></a>Osservazioni

Non è possibile usare le variabili non inizializzate in C#. È possibile inizializzare una variabile con il valore predefinito del relativo tipo. È anche possibile usare il valore predefinito di un tipo per specificare il valore predefinito di un [argomento facoltativo](../../programming-guide/classes-and-structs/named-and-optional-arguments.md#optional-arguments) del metodo.

Usare l'[espressione valore predefinito](../../programming-guide/statements-expressions-operators/default-value-expressions.md) per produrre il valore predefinito di un tipo, come illustrato nell'esempio seguente:

```csharp
int a = default(int);
```

A partire da C# 7.1 è possibile usare il [ valore letterale `default`](../../programming-guide/statements-expressions-operators/default-value-expressions.md#default-literal-and-type-inference) per inizializzare una variabile con il valore predefinito del relativo tipo:

```csharp
int a = default;
```

È anche possibile usare il costruttore senza parametri o il costruttore senza parametri implicito per generare il valore predefinito di un tipo valore, come illustrato nell'esempio seguente. Per altre informazioni sui costruttori, vedere l'articolo [Costruttori](../../programming-guide/classes-and-structs/constructors.md).

```csharp
int a = new int();
```

Il valore predefinito di un qualsiasi [tipo di riferimento](reference-types.md) è `null`. Il valore predefinito di un [tipo nullable](../../programming-guide/nullable-types/index.md) è un'istanza per la quale la proprietà <xref:System.Nullable%601.HasValue%2A> è `false` e la proprietà <xref:System.Nullable%601.Value%2A> non è definita.

## <a name="see-also"></a>Vedere anche

- [Riferimenti per C#](../index.md)
- [Guida per programmatori C#](../../programming-guide/index.md)
- [Parole chiave di C#](index.md)
- [Tipi valore](value-types.md)
- [Tabella dei tipi valore](value-types-table.md)
- [Tabella dei tipi incorporati](built-in-types-table.md)
