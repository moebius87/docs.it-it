---
title: Esecuzione di confronti di stringhe indipendenti dalle impostazioni cultura
ms.date: 08/22/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- String.CompareTo method
- String.Compare method
- string comparison [.NET Framework], culture-insensitive
- strings [.NET Framework], comparing
- culture-insensitive string operations, comparisons
- culture parameter
ms.assetid: abae50ef-32f7-4a50-a540-fd256fd1aed0
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: c7726164e998ea917c8f539b5768aa7e3f1ae12c
ms.sourcegitcommit: 7e129d879ddb42a8b4334eee35727afe3d437952
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66053204"
---
# <a name="performing-culture-insensitive-string-comparisons"></a>Esecuzione di confronti di stringhe indipendenti dalle impostazioni cultura
Per impostazione predefinita, il metodo <xref:System.String.Compare%2A?displayProperty=nameWithType> consente di eseguire confronti dipendenti dalle impostazioni cultura e con distinzione tra maiuscole e minuscole. Questo metodo include anche molti overload che forniscono il parametro `culture` che consente di specificare le impostazioni cultura da usare e il parametro `comparisonType` che consente di specificare le regole di confronto da usare. La chiamata di questi metodi invece dell'overload predefinito rimuove qualsiasi ambiguità sulle regole usate in una particolare chiamata al metodo e chiarisce se un particolare confronto dipende o meno dalle impostazioni cultura.  
  
> [!NOTE]
>  Entrambi gli overload del metodo <xref:System.String.CompareTo%2A?displayProperty=nameWithType> eseguono confronti dipendenti dalle impostazioni cultura e con distinzione tra maiuscole e minuscole. Non è possibile usare questo metodo per eseguire confronti indipendenti dalle impostazioni cultura. Per motivi di chiarezza, in alternativa, si consiglia di usare il metodo <xref:System.String.Compare%2A?displayProperty=nameWithType>.  
  
 Per le operazioni dipendenti dalle impostazioni cultura, specificare il valore di enumerazione <xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType> o <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType> come parametro `comparisonType`. Se si desidera eseguire un confronto dipendente dalle impostazioni cultura usando impostazioni cultura definite diverse da quelle correnti, specificare l'oggetto <xref:System.Globalization.CultureInfo> che rappresenta quelle impostazioni cultura come parametro `culture`.  
  
 I confronti di stringhe indipendenti dalle impostazioni cultura supportati dal metodo <xref:System.String.Compare%2A?displayProperty=nameWithType> sono linguistici (basati sulle convenzioni di ordinamento della lingua inglese) o non linguistici (basati sul valore ordinale dei caratteri nella stringa). La maggior parte di confronti di stringhe indipendenti dalle impostazioni cultura non sono linguistici. Per questi confronti, specificare il valore di enumerazione <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> o <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> come parametro `comparisonType`. Ad esempio, se una decisione relativa alla sicurezza (come ad esempio un confronto del nome utente o della password) è basata sul risultato di un confronto di stringhe, l'operazione dovrebbe essere indipendente dalle impostazioni cultura e non linguistica in modo che il risultato non venga influenzato dalle convenzioni di una lingua o da impostazioni cultura particolari.  
  
 Usare il confronto di stringhe linguistico indipendente dalle impostazioni cultura se si desidera gestire stringhe linguisticamente importanti da più impostazioni cultura in modo coerente. Ad esempio, se l'applicazione visualizza parole che usano più set di caratteri in una casella di riepilogo, è possibile che si desideri visualizzare parole nello stesso ordine indipendentemente dalle impostazioni cultura correnti. Per i confronti linguistici indipendenti dalle impostazioni cultura, .NET Framework definisce una lingua inglese basata sulle convenzioni linguistiche dell'inglese. Per eseguire un confronto linguistico indipendente dalle impostazioni cultura, specificare <xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> o <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> come parametro `comparisonType`.  
  
 Nell'esempio seguente vengono eseguiti due confronti di stringhe indipendenti dalle impostazioni cultura e non linguistici. Il primo, diversamente dal secondo, è con distinzione tra maiuscole e minuscole.  
  
 [!code-csharp[Conceptual.Strings.CultureInsensitiveComparison#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.cultureinsensitivecomparison/cs/cultureinsensitive1.cs#1)]
 [!code-vb[Conceptual.Strings.CultureInsensitiveComparison#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.cultureinsensitivecomparison/vb/cultureinsensitive1.vb#1)]  

È possibile scaricare le [tabelle di ordinamento spessore](https://www.microsoft.com/download/details.aspx?id=10921), un set di file di testo che contengono informazioni sugli spessori dei caratteri usati nelle operazioni di ordinamento e confronto per i sistemi operativi Windows, e la [tabella degli elementi delle regole di confronto Unicode predefinite](https://www.unicode.org/Public/UCA/latest/allkeys.txt), la tabella di ordinamento spessore per Linux e MacOS.

## <a name="see-also"></a>Vedere anche

- <xref:System.String.Compare%2A?displayProperty=nameWithType>
- <xref:System.String.CompareTo%2A?displayProperty=nameWithType>
- [Esecuzione di operazioni sulle stringhe indipendenti dalle impostazioni cultura](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations.md)
- [Procedure consigliate per l'uso delle stringhe](../../../docs/standard/base-types/best-practices-strings.md)
