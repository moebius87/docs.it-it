---
title: Espressioni di confronto
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ec7637a9-01d5-4a95-8bb0-478311cd263b
ms.openlocfilehash: a37e7e3d0759cb3cf17d2b4cbd3dd2e4877ff6c9
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61785398"
---
# <a name="comparison-expressions"></a>Espressioni di confronto
Un'espressione di confronto consente di verificare se un valore costante, un valore di un proprietà o un risultato di un metodo è uguale a, non uguale a, maggiore di o minore di un altro valore. Se un particolare confronto non è valido per [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)], viene generata un'eccezione. Tutti i confronti, sia impliciti che espliciti, richiedono che tutti i componenti possano essere confrontati nell'origine dati. Le espressioni di confronto vengono spesso usate nelle clausole `Where` per limitare i risultati delle query.  
  
 Nell'esempio seguente nella sintassi delle espressioni di query viene illustrata una query che restituisce i risultati in cui il numero dell'ordine di vendita è uguale a "SO43663":  
  
 [!code-csharp[DP L2E Conceptual Examples#RestrictionExpression](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#restrictionexpression)]
 [!code-vb[DP L2E Conceptual Examples#RestrictionExpression](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#restrictionexpression)]  
  
 Nell'esempio seguente nella sintassi delle query basate su metodo viene illustrata una query che restituisce i risultati in cui il numero dell'ordine di vendita è uguale a "SO43663":  
  
 [!code-csharp[DP L2E Conceptual Examples#RestrictionExpression_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#restrictionexpression_mq)]
 [!code-vb[DP L2E Conceptual Examples#RestrictionExpression_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#restrictionexpression_mq)]  
  
 Nell'esempio seguente nella sintassi delle espressioni di query viene illustrata una query che restituisce informazioni sugli ordini di vendita in cui la data di spedizione è l'8 luglio 2001:  
  
 [!code-csharp[DP L2E Conceptual Examples#DateTimeComparison](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#datetimecomparison)]
 [!code-vb[DP L2E Conceptual Examples#DateTimeComparison](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#datetimecomparison)]  
  
 Nell'esempio seguente nella sintassi delle query basate su metodo viene illustrata una query che restituisce informazioni sugli ordini di vendita in cui la data di spedizione è l'8 luglio 2001:  
  
 [!code-csharp[DP L2E Conceptual Examples#DateTimeComparison_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#datetimecomparison_mq)]
 [!code-vb[DP L2E Conceptual Examples#DateTimeComparison_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#datetimecomparison_mq)]  
  
 Le espressioni che producono una costante vengono convertite nel server e non viene eseguito alcun tentativo di valutazione locale. Nell'esempio seguente viene usata un'espressione nella clausola `Where` che produce una costante.  
  
 [!code-csharp[DP L2E Conceptual Examples#ConstantExpression](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#constantexpression)]
 [!code-vb[DP L2E Conceptual Examples#ConstantExpression](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#constantexpression)]  
  
 [!INCLUDE[linq_entities](../../../../../../includes/linq-entities-md.md)] non supporta l'uso di una classe utente come costante. Un riferimento a una proprietà in una classe utente è tuttavia considerato una costante e viene convertito in un'espressione costante dell'albero dei comandi ed eseguito nell'origine dati.  
  
 [!code-csharp[DP L2E Conceptual Examples#MyClass](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#myclass)]
 [!code-vb[DP L2E Conceptual Examples#MyClass](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#myclass)]  
  
 [!code-csharp[DP L2E Conceptual Examples#PropertyAsConstant](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#propertyasconstant)]
 [!code-vb[DP L2E Conceptual Examples#PropertyAsConstant](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#propertyasconstant)]  
  
 I metodi che restituiscono un'espressione costante non sono supportati. Nell'esempio seguente è contenuto un metodo nella clausola `Where` che restituisce una costante. Questo esempio provoca la generazione di un'eccezione in fase di runtime.  
  
 [!code-csharp[DP L2E Conceptual Examples#MethodAsConstantFails](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#methodasconstantfails)]
 [!code-vb[DP L2E Conceptual Examples#MethodAsConstantFails](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#methodasconstantfails)]  
  
## <a name="see-also"></a>Vedere anche

- [Espressioni in query di LINQ to Entities](../../../../../../docs/framework/data/adonet/ef/language-reference/expressions-in-linq-to-entities-queries.md)
