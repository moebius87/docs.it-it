---
title: Cenni preliminari sulla grafica
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [Windows Forms], using managed interface
- graphics [Windows Forms], about graphics
ms.assetid: a602aef8-a8c8-4c36-9816-74e7bad96a68
ms.openlocfilehash: 927fc327d9ad42cd3a99af207d04efbc520df8b5
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2019
ms.locfileid: "64645708"
---
# <a name="overview-of-graphics"></a>Cenni preliminari sulla grafica
[!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] è un'application programming interface (API) che costituisce il sottosistema del sistema operativo Microsoft Windows. [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] è responsabile della visualizzazione di informazioni sugli schermi e stampanti. Come suggerisce il nome, [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] è il successore di [!INCLUDE[ndptecgdi](../../../../includes/ndptecgdi-md.md)], ovvero le Graphics Device Interface incluse nelle versioni precedenti di Windows.  
  
## <a name="managed-class-interface"></a>Interfaccia di classe gestita  
 Il [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] API viene esposta tramite un set di classi distribuito come codice gestito. Questo set di classi viene chiamato il *interfaccia di classe gestita* a [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)]. Di seguito sono elencati gli spazi dei nomi che costituiscono l'interfaccia di classe gestita:  
  
- <xref:System.Drawing>  
  
- <xref:System.Drawing.Drawing2D>  
  
- <xref:System.Drawing.Imaging>  
  
- <xref:System.Drawing.Text>  
  
- <xref:System.Drawing.Printing>  
  
 Con una Graphics Device Interface, ad esempio [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)], è possibile visualizzare informazioni su un monitor o una stampante senza doversi preoccupare dei dettagli relativi a un dispositivo di visualizzazione particolare. Il programmatore effettua chiamate ai metodi forniti dalle classi [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)]. Tali metodi, a loro volta, effettuano le chiamate appropriate a specifici driver di dispositivo. [!INCLUDE[ndptecgdiplus](../../../../includes/ndptecgdiplus-md.md)] isola l'applicazione dall'hardware grafico. È questo isolamento che consente a un programmatore di creare applicazioni indipendenti dal dispositivo.  
  
## <a name="see-also"></a>Vedere anche

- [Cenni preliminari sulla grafica](graphics-overview-windows-forms.md)
