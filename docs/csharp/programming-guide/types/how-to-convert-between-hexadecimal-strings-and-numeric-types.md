---
title: 'Procedura: Eseguire la conversione tra stringhe esadecimali e tipi numerici - Guida per programmatori C#'
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- hexadecimal strings [C#], converting to numeric type
- conversions [C#], hexidecimal strings
- strings [C#], converting hexadecimal strings
- hexadecimal strings [C#]
ms.assetid: 7115c49f-7d1d-40c3-8bd9-aae0cc1d46b6
ms.openlocfilehash: 99d30d6c2b50569312ff2d732a34020ab29ce81c
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64595068"
---
# <a name="how-to-convert-between-hexadecimal-strings-and-numeric-types-c-programming-guide"></a>Procedura: Eseguire la conversione tra stringhe esadecimali e tipi numerici (Guida per programmatori C#)
In questi esempi viene mostrato come effettuare le seguenti attività:  
  
- Ottenere il valore esadecimale di ogni carattere in un oggetto [string](../../../csharp/language-reference/keywords/string.md).  
  
- Ottenere l'oggetto [char](../../../csharp/language-reference/keywords/char.md) che corrisponde a ogni valore in una stringa esadecimale.  
  
- Convertire un oggetto `string` esadecimale in un oggetto [int](../../../csharp/language-reference/keywords/int.md).  
  
- Convertire un oggetto `string` esadecimale in un oggetto [float](../../../csharp/language-reference/keywords/float.md).  
  
- Convertire una matrice di [byte](../../../csharp/language-reference/keywords/byte.md) in un oggetto `string` esadecimale.  
  
## <a name="example"></a>Esempio  
 Questo esempio restituisce il valore esadecimale di ogni carattere in un oggetto `string`. Prima viene analizzato l'oggetto `string` in una matrice di caratteri. Poi viene chiamato <xref:System.Convert.ToInt32%28System.Char%29> in ogni carattere per ottenere il rispettivo valore numerico. Il numero viene infine formattato come esadecimale in un oggetto `string`.  
  
 [!code-csharp[csProgGuideTypes#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#30)]  
  
## <a name="example"></a>Esempio  
 Questo esempio analizza un oggetto `string` di valori esadecimali e restituisce il carattere corrispondente a ogni valore esadecimale. Prima viene chiamato il metodo [Split(Char\[\])](xref:System.String.Split(System.Char[])) per ottenere ogni valore esadecimale come singolo oggetto `string` in una matrice. Poi viene chiamato <xref:System.Convert.ToInt32%28System.String%2CSystem.Int32%29> per convertire il valore esadecimale in un valore decimale rappresentato come [int](../../../csharp/language-reference/keywords/int.md). Per ottenere il carattere corrispondente al codice di tale carattere vengono illustrate due modalità diverse. La prima tecnica usa `string` che restituisce il carattere corrispondente all'argomento Integer come <xref:System.Char.ConvertFromUtf32%28System.Int32%29>. La seconda tecnica esegue il cast in modo esplicito del valore `int` a un valore [char](../../../csharp/language-reference/keywords/char.md).  
  
 [!code-csharp[csProgGuideTypes#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#31)]  
  
## <a name="example"></a>Esempio  
 Questo esempio illustra un altro modo per convertire un valore esadecimale `string` in un Integer, chiamando il metodo <xref:System.Int32.Parse%28System.String%2CSystem.Globalization.NumberStyles%29>.  
  
 [!code-csharp[csProgGuideTypes#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#32)]  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra come convertire un tipo `string` esadecimale in [float](../../../csharp/language-reference/keywords/float.md) usando la classe <xref:System.BitConverter?displayProperty=nameWithType> e il metodo <xref:System.UInt32.Parse%2A?displayProperty=nameWithType>.  
  
 [!code-csharp[csProgGuideTypes#39](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#39)]  
  
## <a name="example"></a>Esempio  
 L'esempio seguente illustra come convertire una matrice [byte](../../../csharp/language-reference/keywords/byte.md) in una stringa esadecimale tramite la classe <xref:System.BitConverter?displayProperty=nameWithType>.  
  
 [!code-csharp[csProgGuideTypes#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#38)]  
  
## <a name="see-also"></a>Vedere anche

- [Standard Numeric Format Strings](../../../standard/base-types/standard-numeric-format-strings.md)
- [Tipi](../../../csharp/programming-guide/types/index.md)
- [Procedura: Determinare se una stringa rappresenta un valore numerico](../../../csharp/programming-guide/strings/how-to-determine-whether-a-string-represents-a-numeric-value.md)
