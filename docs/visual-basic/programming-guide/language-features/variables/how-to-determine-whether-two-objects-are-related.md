---
title: 'Procedura: Determinare se due oggetti sono correlati (Visual Basic)'
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], Visual Basic objects
- objects [Visual Basic], inheritance
- object variables [Visual Basic], determining relation
ms.assetid: da002e3f-6616-4bad-a229-f842d06652bb
ms.openlocfilehash: f59e00d80d28fc4bf24874d25b5c12643649c834
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61769070"
---
# <a name="how-to-determine-whether-two-objects-are-related-visual-basic"></a>Procedura: Determinare se due oggetti sono correlati (Visual Basic)
È possibile confrontare due oggetti per stabilire la relazione, se presente, tra le classi da cui vengono creati. Il <xref:System.Type.IsInstanceOfType%2A> metodo per il <xref:System.Type?displayProperty=nameWithType> classe restituisce `True` se la classe specificata eredita dalla classe corrente o se il tipo corrente è un'interfaccia supportata dalla classe specificata.  
  
### <a name="to-determine-if-one-object-inherits-from-another-objects-class-or-interface"></a>Per determinare se un oggetto eredita da un altro oggetto classe o interfaccia  
  
1. Per l'oggetto si ritiene che potrebbero essere del tipo di base, richiamare il <xref:System.Object.GetType%2A> (metodo).  
  
2. Nel <xref:System.Type?displayProperty=nameWithType> oggetto restituito da <xref:System.Object.GetType%2A>, richiamare il <xref:System.Type.IsInstanceOfType%2A> (metodo).  
  
3. Nell'elenco di argomenti per <xref:System.Type.IsInstanceOfType%2A>, specificare l'oggetto si ritiene che potrebbe essere del tipo derivato.  
  
     <xref:System.Type.IsInstanceOfType%2A> Restituisce `True` se il tipo di argomento eredita dal <xref:System.Type?displayProperty=nameWithType> tipo di oggetto.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente determina se un oggetto rappresenta una classe derivata dalla classe dell'oggetto.  
  
```  
Public Class baseClass  
End Class  
Public Class derivedClass : Inherits baseClass  
End Class  
Public Class testTheseClasses  
    Public Sub seeIfRelated()  
        Dim baseObj As Object = New baseClass()  
        Dim derivedObj As Object = New derivedClass()  
        Dim related As Boolean  
        related = baseObj.GetType().IsInstanceOfType(derivedObj)  
        MsgBox(CStr(related))  
    End Sub  
End Class  
```  
  
 Si noti la posizione di due variabili oggetto nella chiamata a imprevisto <xref:System.Type.IsInstanceOfType%2A>. Il tipo base previsto è utilizzato per generare il <xref:System.Type?displayProperty=nameWithType> classe e il tipo derivato previsto viene passata come argomento per il <xref:System.Type.IsInstanceOfType%2A> (metodo).  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Object.GetType%2A>
- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Type.IsInstanceOfType%2A>
- [Tipo di dati Object](../../../../visual-basic/language-reference/data-types/object-data-type.md)
- [Variabili oggetto](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [Valori di variabili oggetto](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
- [Procedura: Determinare se due oggetti sono identici](../../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-identical.md)
