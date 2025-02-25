---
title: 'Procedura: Impostare le colonne come di sola lettura nel controllo DataGridView di Windows Forms usando la finestra di progettazione'
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, columns
- DataGridView control [Windows Forms], read-only columns
- data [Windows Forms], displaying
- columns [Windows Forms], read-only
ms.assetid: b4ef7a75-ab33-4ee3-b2cf-201530e454e9
ms.openlocfilehash: a735b9bef9f9e3488941e05b2aa9444e6ecdc4b8
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62012880"
---
# <a name="how-to-make-columns-read-only-in-the-windows-forms-datagridview-control-using-the-designer"></a>Procedura: Impostare le colonne come di sola lettura nel controllo DataGridView di Windows Forms usando la finestra di progettazione
Per impostazione predefinita, gli utenti possono modificare il testo e i dati numerici visualizzati nei moduli di Windows <xref:System.Windows.Forms.DataGridView> controllo. Se si desidera visualizzare i dati che non sono utilizzati per la modifica, è necessario apportare le colonne che contengono i dati di sola lettura. Per informazioni su come rendere il controllo interamente di sola lettura, vedere [come: Impedire l'aggiunta di riga o eliminazione in Windows il controllo DataGridView form usando la finestra di progettazione](prevent-row-addition-and-deletion-in-the-datagrid-using-the-designer.md).  
  
 La procedura seguente richiede un **applicazione di Windows** progetto con un form contenente un <xref:System.Windows.Forms.DataGridView> controllo. Per informazioni sulla configurazione di un progetto di questo tipo, vedere [come: Creare un progetto Windows Forms application](/visualstudio/ide/step-1-create-a-windows-forms-application-project) e [come: Aggiungere controlli a un Windows Form](how-to-add-controls-to-windows-forms.md).  
  
> [!NOTE]
>  Le finestre di dialogo e i comandi di menu visualizzati potrebbero essere diversi da quelli descritti nella Guida a seconda delle impostazioni attive o dell'edizione del programma. Per modificare le impostazioni, scegliere **Importa/Esporta impostazioni** dal menu **Strumenti** . Per altre informazioni, vedere [Personalizzare l'IDE di Visual Studio](/visualstudio/ide/personalizing-the-visual-studio-ide).  
  
### <a name="to-make-a-column-read-only-by-using-the-designer"></a>Per impostare una colonna di sola lettura usando la finestra di progettazione  
  
1. Fare clic sul glifo dello smart tag (![glifo Smart Tag](./media/vs-winformsmttagglyph.gif "VS_WinFormSmtTagGlyph")) nell'angolo superiore destro della <xref:System.Windows.Forms.DataGridView> controllare e quindi selezionare **Modifica colonne**.  
  
2. Selezionare una colonna dal **colonne selezionate** elenco.  
  
3. Nel **le proprietà delle colonne** griglia, impostare il <xref:System.Windows.Forms.DataGridViewColumn.ReadOnly%2A> proprietà `true`.  
  
    > [!NOTE]
    >  È anche possibile rendere una colonna di sola lettura quando si aggiunge, selezionando il **Read Only** casella di controllo nella **Aggiungi colonna** nella finestra di dialogo.  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn.ReadOnly%2A?displayProperty=nameWithType>
- [Procedura: Aggiungere e rimuovere colonne nel controllo DataGridView Windows Form usando la finestra di progettazione](add-and-remove-columns-in-the-datagrid-using-the-designer.md)
- [Procedura: Impedire l'aggiunta di riga e l'eliminazione nel controllo DataGridView Windows Form usando la finestra di progettazione](prevent-row-addition-and-deletion-in-the-datagrid-using-the-designer.md)
- [Procedura: Creare un progetto di Windows Forms Application](/visualstudio/ide/step-1-create-a-windows-forms-application-project)
- [Procedura: Aggiungere controlli a un Windows Form](how-to-add-controls-to-windows-forms.md)
