---
title: 'Procedura: Inviare modifiche al database'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c7cba174-9d40-491d-b32c-f2d73b7e9eab
ms.openlocfilehash: 222ce575d9e977cc8b68862385b4a1b147c6394a
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61902694"
---
# <a name="how-to-submit-changes-to-the-database"></a>Procedura: Inviare modifiche al database
Indipendentemente da quante modifiche si apportano agli oggetti, queste vengono applicate solo alle repliche in memoria. Le modifiche non vengono apportate ai dati effettivi nel database e non saranno trasmesse al server finché non si chiama in modo esplicito <xref:System.Data.Linq.DataContext.SubmitChanges%2A> su <xref:System.Data.Linq.DataContext>.  
  
 Quando si effettua questa chiamata, <xref:System.Data.Linq.DataContext> tenta di convertire le modifiche in comandi SQL equivalenti. È possibile usare una logica personalizzata per eseguire l'override di queste azioni, ma l'ordine di invio viene gestito da un servizio del <xref:System.Data.Linq.DataContext> noto come il *processore delle modifiche*. Di seguito viene riportata la sequenza degli eventi:  
  
1. Quando si chiama <xref:System.Data.Linq.DataContext.SubmitChanges%2A>, [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] esamina il set di oggetti conosciuti per determinare se a tali oggetti sono state associate nuove istanze. In caso affermativo, queste nuove istanze vengono aggiunte al set di oggetti registrati.  
  
2. Tutti gli oggetti con modifiche in sospeso vengono ordinati in una sequenza di oggetti in base alle reciproche dipendenze. Gli oggetti le cui modifiche dipendono da altri oggetti vengono ordinati in sequenza dopo le relative dipendenze.  
  
3. Immediatamente prima della trasmissione di una qualsiasi modifica effettiva, [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] avvia una transazione per incapsulare la serie di singoli comandi.  
  
4. Le modifiche agli oggetti vengono convertite una alla volta in comandi SQL e inviate al server.  
  
 Qualsiasi errore rilevato dal database in questa fase causa l'interruzione del processo di invio e la generazione di un'eccezione. Viene eseguito il rollback di tutte le modifiche al database come se non fosse mai stato eseguito alcun invio. In <xref:System.Data.Linq.DataContext> è tuttavia ancora presente una registrazione completa di tutte le modifiche, pertanto è possibile tentare di correggere il problema e chiamare nuovamente <xref:System.Data.Linq.DataContext.SubmitChanges%2A>, come nell'esempio di codice che segue.  
  
## <a name="example"></a>Esempio  
 Quando la transazione relativa alla sottomissione viene completata correttamente, <xref:System.Data.Linq.DataContext> accetta le modifiche agli oggetti ignorando le informazioni di rilevamento delle modifiche.  
  
 [!code-csharp[DLinqSubmittingChanges#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSubmittingChanges/cs/Program.cs#1)]
 [!code-vb[DLinqSubmittingChanges#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSubmittingChanges/vb/Module1.vb#1)]  
  
## <a name="see-also"></a>Vedere anche

- [Procedura: Rilevare e risolvere gli invii in conflitto](../../../../../../docs/framework/data/adonet/sql/linq/how-to-detect-and-resolve-conflicting-submissions.md)
- [Procedura: Gestire i conflitti di modifiche](../../../../../../docs/framework/data/adonet/sql/linq/how-to-manage-change-conflicts.md)
- [Download di database di esempio](../../../../../../docs/framework/data/adonet/sql/linq/downloading-sample-databases.md)
- [Creazione e invio di modifiche dei dati](../../../../../../docs/framework/data/adonet/sql/linq/making-and-submitting-data-changes.md)
