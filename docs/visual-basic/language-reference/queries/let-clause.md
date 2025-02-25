---
title: Clausola Let (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.QueryLet
helpviewer_keywords:
- queries [Visual Basic], Let
- Let clause [Visual Basic]
- Let statement [Visual Basic]
ms.assetid: 981aa516-16eb-4c53-b1f1-5aa3e82f316e
ms.openlocfilehash: ff298f001a2d865446436e8099a2fbbef593a00a
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62054198"
---
# <a name="let-clause-visual-basic"></a>Clausola Let (Visual Basic)
Calcola un valore e lo assegna a una nuova variabile all'interno della query.  
  
## <a name="syntax"></a>Sintassi  
  
```  
Let variable = expression [, ...]  
```  
  
## <a name="parts"></a>Parti  
  
|Termine|Definizione|  
|---|---|  
|`variable`|Obbligatorio. Un alias che può essere utilizzato per fare riferimento ai risultati dell'espressione fornita.|  
|`expression`|Obbligatorio. Un'espressione che verrà valutata e assegnata alla variabile specificata.|  
  
## <a name="remarks"></a>Note  
 Il `Let` clausola consente di calcolare i valori per ogni risultato della query e farvi riferimento utilizzando un alias. L'alias può essere utilizzato in altre clausole, ad esempio il `Where` clausola. Il `Let` clausola consente di creare un'istruzione di query che è più facile da leggere in quanto è possibile specificare un alias per una clausola dell'espressione inclusa nella query e sostituire l'alias ogni volta che viene utilizzata la clausola dell'espressione.  
  
 È possibile includere un numero qualsiasi di `variable` e `expression` assegnazioni nel `Let` clausola. Separare ogni assegnazione con una virgola (,).  
  
## <a name="example"></a>Esempio  
 Il codice seguente viene illustrato come utilizzare il `Let` clausola per calcolare un 10% di sconto sui prodotti.  
  
 [!code-vb[VbSimpleQuerySamples#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#16)]  
  
## <a name="see-also"></a>Vedere anche

- [Introduzione a LINQ in Visual Basic](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [Query](../../../visual-basic/language-reference/queries/index.md)
- [Clausola Select](../../../visual-basic/language-reference/queries/select-clause.md)
- [Clausola From](../../../visual-basic/language-reference/queries/from-clause.md)
- [Clausola Where](../../../visual-basic/language-reference/queries/where-clause.md)
