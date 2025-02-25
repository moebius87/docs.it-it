---
title: 'Procedura: Dichiarare eventi personalizzati per proteggere la memoria (Visual Basic)'
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 87ebee87-260c-462f-979c-407874debd19
ms.openlocfilehash: e4132f51f4dd85ad964042d05f7c5bc0a2e6e3cd
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61973160"
---
# <a name="how-to-declare-custom-events-to-conserve-memory-visual-basic"></a>Procedura: Dichiarare eventi personalizzati per proteggere la memoria (Visual Basic)
Esistono diversi casi quando è importante che un'applicazione mantenere ridotto l'utilizzo della memoria. Eventi personalizzati consentono all'applicazione di utilizzo della memoria solo per gli eventi che gestisce.  
  
 Per impostazione predefinita, quando una classe dichiara un evento, il compilatore alloca memoria per un campo archiviare le informazioni sull'evento. Se una classe dispone di molti eventi inutilizzati, inutilmente occupano memoria.  
  
 Invece di usare l'implementazione predefinita di eventi disponibili in Visual Basic, è possibile utilizzare gli eventi personalizzati per gestire l'utilizzo della memoria più attentamente.  
  
## <a name="example"></a>Esempio  
 In questo esempio, la classe usa un'istanza del <xref:System.ComponentModel.EventHandlerList> (classe), archiviate nel `Events` campo, per archiviare le informazioni sugli eventi in uso. Il <xref:System.ComponentModel.EventHandlerList> è una classe di elenco ottimizzata progettata per contenere i delegati.  
  
 Tutti gli eventi della classe utilizzano il `Events` campo per tenere traccia di quali metodi sono la gestione di ogni evento.  
  
 [!code-vb[VbVbalrEvents#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#22)]  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.ComponentModel.EventHandlerList>
- [Eventi](../../../../visual-basic/programming-guide/language-features/events/index.md)
- [Procedura: Dichiarare eventi personalizzati per evitare il blocco](../../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)
