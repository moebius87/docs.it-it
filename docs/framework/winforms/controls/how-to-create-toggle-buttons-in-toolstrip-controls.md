---
title: 'Procedura: Creare pulsanti interruttore nei controlli ToolStrip'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- toggle buttons [Windows Forms], creating
- examples [Windows Forms], toolbars
- ToolStrip control [Windows Forms], creating toggle buttons
ms.assetid: d9c197df-4c65-43f2-beee-b68b52b2befc
ms.openlocfilehash: 21da5564bfeec01d448c23d3e734bdd16fc1566b
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64666426"
---
# <a name="how-to-create-toggle-buttons-in-toolstrip-controls"></a>Procedura: Creare pulsanti interruttore nei controlli ToolStrip
Quando un utente fa clic su un pulsante Mostra/Nascondi, viene visualizzato incassato e mantiene tale aspetto fino a quando l'utente fa clic sul pulsante nuovo.  
  
### <a name="to-create-a-toggling-toolstripbutton"></a>Per creare un ToolStripButton  
  
- Usare codice simile al codice seguente. Questo codice presuppone che il form contenga un <xref:System.Windows.Forms.ToolStrip> controllo e che il <xref:System.Windows.Forms.ToolStrip.Items%2A> raccolta contiene un <xref:System.Windows.Forms.ToolStripButton> chiamato `toolStripButton1`. Si presume anche che un gestore eventi chiamato `toolStripButton1_CheckedChanged`.  
  
    ```vb  
    toolStripButton1.CheckOnClick = True  
    toolStripButton1.CheckedChanged AddressOf _  
    EventHandler(toolStripButton1_CheckedChanged);  
    ```  
  
    ```csharp  
    toolStripButton1.CheckOnClick = true;  
    toolStripButton1.CheckedChanged += new _  
    EventHandler(toolStripButton1_CheckedChanged);  
    ```  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Windows.Forms.ToolStripButton>
- [Panoramica sul controllo ToolStrip](toolstrip-control-overview-windows-forms.md)
