---
title: -main
ms.date: 03/13/2018
helpviewer_keywords:
- main compiler option [Visual Basic]
- /main compiler option [Visual Basic]
- -main compiler option [Visual Basic]
ms.assetid: 83fc339d-6652-415d-b205-b5133319b5b0
ms.openlocfilehash: fd6240faf702ccb5e543bfd6a7779284f38d8850
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2019
ms.locfileid: "61793913"
---
# <a name="-main"></a>-main
Specifica la classe o il modulo che contiene la procedura `Sub Main`.  
  
## <a name="syntax"></a>Sintassi  
  
```  
-main:location  
```  
  
## <a name="arguments"></a>Argomenti  
 `location`  
 Obbligatorio. Il nome della classe o modulo che contiene il `Sub Main` procedure da chiamare all'avvio del programma. Ciò potrebbe essere nel formato **-/Main: module** oppure **-main:namespace.module**.  
  
## <a name="remarks"></a>Note  
 Usare questa opzione quando si crea un file eseguibile o un programma eseguibile Windows. Se il **-principale** opzione viene omessa, il compilatore cerca condiviso valido `Sub Main` in tutte le classi pubbliche e moduli.  
  
 Visualizzare [routine Main in Visual Basic](../../../visual-basic/programming-guide/program-structure/main-procedure.md) per una descrizione delle varie forme del `Main` procedure.  
  
 Quando `location` è una classe che eredita da <xref:System.Windows.Forms.Form>, il compilatore fornisce un valore predefinito `Main` routine che avvia l'applicazione se la classe non ha alcun `Main` procedure. Ciò consente di compilare il codice nella riga di comando che è stata creata nell'ambiente di sviluppo.  
  
 [!code-vb[VbVbalrCompiler#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/Class1.vb#16)]  
  
### <a name="to-set--main-in-the-visual-studio-integrated-development-environment"></a>Per impostare - principale nell'ambiente di sviluppo integrato di Visual Studio  
  
1. Selezionare un progetto in **Esplora soluzioni**. Scegliere **Proprietà** dal menu **Progetto**.  
  
2. Fare clic sulla scheda **Applicazione** .  
  
3. Assicurarsi che il **Abilita framework applicazione** casella di controllo è deselezionata.  
  
4. Modificare il valore di **oggetto di avvio** casella.  
  
## <a name="example"></a>Esempio  
 Il codice seguente Compila `T2.vb` e `T3.vb`, specificando che il `Sub Main` sarà disponibili nella procedura di `Test2` classe.  
  
```console
vbc t2.vb t3.vb -main:Test2  
```  
  
## <a name="see-also"></a>Vedere anche

- [Compilatore della riga di comando di Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)
- [-target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [Esempi di righe di comando di compilazione](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
- [Routine Main in Visual Basic](../../../visual-basic/programming-guide/program-structure/main-procedure.md)
