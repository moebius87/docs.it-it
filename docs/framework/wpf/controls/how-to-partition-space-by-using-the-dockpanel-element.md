---
title: "Procedura: Partizionare lo spazio usando l'elemento DockPanel"
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- controls [WPF], DockPanel
- DockPanel control [WPF], partitioning space
- partitioning space [WPF]
ms.assetid: a219b9e5-b205-4438-89b5-0a137ac463ab
ms.openlocfilehash: ab51270644bf370944ebc933c765b40c528681c5
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "62052183"
---
# <a name="how-to-partition-space-by-using-the-dockpanel-element"></a>Procedura: Partizionare lo spazio usando l'elemento DockPanel
L'esempio seguente crea una semplice [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] framework usando un <xref:System.Windows.Controls.DockPanel> elemento. Il <xref:System.Windows.Controls.DockPanel> partiziona lo spazio disponibile per i relativi elementi figlio.  
  
## <a name="example"></a>Esempio  
 Questo esempio Usa la <xref:System.Windows.Controls.DockPanel.Dock%2A> proprietà, ovvero una proprietà associata, per ancorare due identici <xref:System.Windows.Controls.Border> elementi nel <xref:System.Windows.Controls.Dock.Top> dello spazio partizionato. Una terza <xref:System.Windows.Controls.Border> per l'elemento è ancorato il <xref:System.Windows.Controls.Dock.Left>, con una larghezza di 200 pixel. Un quarto <xref:System.Windows.Controls.Border> è ancorato il <xref:System.Windows.Controls.Dock.Bottom> dello schermo. L'ultimo <xref:System.Windows.Controls.Border> elemento riempie automaticamente lo spazio rimanente.  
  
 [!code-cpp[DockPanelOvwSample#1](~/samples/snippets/cpp/VS_Snippets_Wpf/DockPanelOvwSample/CPP/DockPanel_Ovw_Sample.cpp#1)]
 [!code-csharp[DockPanelOvwSample#1](~/samples/snippets/csharp/VS_Snippets_Wpf/DockPanelOvwSample/CSharp/DockPanel_Ovw_Sample.cs#1)]
 [!code-vb[DockPanelOvwSample#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DockPanelOvwSample/VisualBasic/dockpanel_vb.vb#1)]
 [!code-xaml[DockPanelOvwSample#1](~/samples/snippets/xaml/VS_Snippets_Wpf/DockPanelOvwSample/XAML/default.xaml#1)]  
  
> [!NOTE]
>  Per impostazione predefinita, l'ultimo elemento figlio di un <xref:System.Windows.Controls.DockPanel> elemento riempie il rimanente spazio non allocato. Se non si desidera questo comportamento, impostare `LastChildFill="False"`.  
  
 L'applicazione compilata crea una nuova interfaccia utente analoga alla seguente.  
  
 ![Scenario DockPanel tipico.](./media/panel-intro-dockpanel.PNG "panel_intro_dockpanel")  
  
## <a name="see-also"></a>Vedere anche

- <xref:System.Windows.Controls.DockPanel>
- [Cenni preliminari sugli elementi Panel](panels-overview.md)
